# Historico de mudancas no servidor

## 14/11/2026

**Manutencao do sistema (sem derrubar Bots Central/aval-agent)**
- 66 pacotes atualizados com `needrestart` suspenso; `docker.io`, `containerd`, `runc`, `tailscale` travados durante a atualizacao.
- Docker: `live-restore: true` em `/etc/docker/daemon.json` (aplicado com `reload`, sem parar containers). Backup em `/etc/docker/daemon.json.bak-2026-11-14`.
- `containerd` 2.2.2-0ubuntu1.2 e Tailscale 1.104.7 atualizados com containers rodando. Holds removidos.
- Servicos auxiliares reiniciados (nao criticos).
- Reboot para o kernel 7.0.0-43. Tudo voltou sozinho.
- `systemd-networkd-wait-online` com `--any --timeout=30` (boot de ~2min20 para ~25 s).

**Novos servicos**
- Radar News Bot (`~/radar-news-bot`, Docker, Ollama proprio). Repo `Voide-Kali/radar-news-bot`, deploy key so leitura.
- Immich v3 (`~/immich-app`), fotos em `/mnt/storage/immich/library`. Funnel 8443.
- Pi-hole (`~/pihole`), DNS em `192.168.1.18:53`. Painel via serve 8445 (tailnet).
- Nextcloud 34 (`~/nextcloud`), arquivos em `/mnt/storage/nextcloud/data`. Funnel 10000.

**Kali (cliente)**
- `tailscaled` habilitado no boot.
- `/etc/hosts` com `aval.tail5e5757.ts.net` apontando para os IPs do Tailscale.
