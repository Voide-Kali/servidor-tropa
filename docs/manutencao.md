# Manutencao

## Checklist de saude (antes e depois de tudo)

```bash
docker ps --format '{{.Names}}\t{{.Status}}' | sort
systemctl is-active aval-agent docker tailscaled
tailscale serve status
df -h / /mnt/storage
```

## Atualizar o sistema sem derrubar nada

O Docker esta com **live-restore ligado** (`/etc/docker/daemon.json`), entao atualizar `docker.io`/`containerd` **nao derruba os containers**.

```bash
tmux new -s manut          # se o SSH cair, a atualizacao continua (voltar: tmux attach -t manut)
sudo apt update
apt list --upgradable
sudo NEEDRESTART_SUSPEND=1 DEBIAN_FRONTEND=noninteractive apt-get -o Dpkg::Options::=--force-confold upgrade -y
```
- `NEEDRESTART_SUSPEND=1` impede que servicos sejam reiniciados automaticamente.
- Atualizar o pacote `tailscale` derruba a sessao SSH por ~10-30 s (os bots nao caem).

Depois, ver quais servicos estao com bibliotecas antigas (so lista, nao reinicia):
```bash
sudo needrestart -r l -b | grep -E 'NEEDRESTART-(SVC|KCUR|KEXP)'
```

Seguros de reiniciar (nao afetam bots nem rede): avahi-daemon, bluetooth, chrony, fwupd, polkit, smartmontools, udisks2, upower, unattended-upgrades, ModemManager, multipathd, networkd-dispatcher, libvirtd, virtlockd, virtlogd, ssh.

**Nao reiniciar a quente**: `wpa_supplicant` / `netplan-wpa-wlp3s0` (derruba a rede, o servidor esta no Wi-Fi), `dbus`, `systemd-logind`. Esses so com reboot.

## Reboot seguro

Leva ~1-2 min de indisponibilidade. Tudo volta sozinho (`restart: unless-stopped` e servicos `enabled`).

Antes:
```bash
lsblk -o NAME,TYPE,FSTYPE | grep -i crypt || echo "sem criptografia"   # nao pode pedir senha no boot
systemctl is-enabled aval-agent docker containerd tailscaled ssh
ls /boot/vmlinuz-*
```
Reboot:
```bash
sudo reboot
```
Do seu computador, esperar voltar:
```bash
until tailscale ping -c 1 --timeout 3s 100.87.201.41 >/dev/null 2>&1; do sleep 10; done; echo VOLTOU
```
Logo apos voltar, o login pode dizer "System is booting up" por ~1 min: normal.

## Boot

`systemd-networkd-wait-online` foi ajustado para esperar **qualquer** rede por no maximo 30 s (antes esperava a porta de cabo vazia por 2 min):
```
/etc/systemd/system/systemd-networkd-wait-online.service.d/99-wait-any.conf
```
Ver tempo de boot: `systemd-analyze; systemd-analyze blame | head`.

## Logs

```bash
docker logs --tail 50 <container>
journalctl -u <servico> -n 50 --no-pager
journalctl -b -p err --no-pager | tail -50     # erros do boot atual
```
Logs do Docker com rotacao (10 MB x 3) configurada em `/etc/docker/daemon.json`.

## Temperatura

```bash
sensors | grep -Ei 'package|core'
```
A tela de login mostra 126 C: e um sensor sem nada ligado (`temp4`/`temp8`). A CPU real fica ~40 C.
