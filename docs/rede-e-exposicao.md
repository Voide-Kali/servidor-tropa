# Rede e exposicao

## Rede local

| Item | Valor |
|---|---|
| Interface | `wlp3s0` (Wi-Fi). A porta de cabo `enp2s0` existe mas esta sem cabo |
| IP | `192.168.1.18/24`, fixo (`dhcp4: false`) |
| Gateway | `192.168.1.1` |
| DNS do proprio servidor | `1.1.1.1`, `1.0.0.1`, `8.8.8.8` (via `systemd-resolved`, em `127.0.0.53`) |
| Configuracao | `/etc/netplan/00-installer-config.yaml` (contem a senha do Wi-Fi: **nao copiar para lugar nenhum**) |

## Tailscale (serve / funnel)

O Tailscale publica os servicos com HTTPS valido em `aval.tail5e5757.ts.net`.

- **Funnel** = aberto para a internet. So aceita as portas **443, 8443 e 10000**.
- **Serve** = so para aparelhos da sua tailnet. Aceita outras portas.

| Porta | Tipo | Destino no servidor | Servico |
|---|---|---|---|
| 443 | **Funnel (internet)** | `127.0.0.1:8082` | Bots Central (web) |
| 8443 | **Funnel (internet)** | `127.0.0.1:2283` | Immich |
| 10000 | **Funnel (internet)** | `127.0.0.1:8095` | Nextcloud |
| 8445 | Serve (tailnet) | `192.168.1.18:8090` | Painel do Pi-hole |

Ver o estado atual:
```bash
tailscale serve status
tailscale funnel status
```

Exemplos de mudanca:
```bash
# fechar um servico da internet e deixar so na tailnet
sudo tailscale funnel --https=8443 off
sudo tailscale serve --bg --https=8443 http://127.0.0.1:2283

# abrir para a internet (so 443, 8443 ou 10000)
sudo tailscale funnel --bg --https=10000 http://127.0.0.1:8095

# remover de vez
sudo tailscale serve --https=8445 off
```

## Portas abertas no servidor

Todos os servicos web escutam so em `127.0.0.1` e saem pelo Tailscale. A unica excecao e o Pi-hole, que precisa atender a rede de casa:

| Endereco | Servico |
|---|---|
| `192.168.1.18:53` tcp/udp | DNS do Pi-hole |
| `192.168.1.18:8090` | Painel do Pi-hole (rede de casa) |
| `127.0.0.1:8082` | Bots Central web |
| `127.0.0.1:2283` | Immich |
| `127.0.0.1:8095` | Nextcloud |
| `0.0.0.0:22` | SSH |
| `192.168.122.1:53` | dnsmasq da rede de VMs (libvirt) |

## Problema de DNS com Funnel (importante)

Como o `aval` tem Funnel, o nome `aval.tail5e5757.ts.net` tambem existe no DNS publico, apontando para os servidores do Funnel (IPv6 `2607:f740:...`). Um computador que **nao usa o MagicDNS do Tailscale** vai pelo caminho publico e **nao consegue abrir as portas que sao so da tailnet** (erro `ERR_CONNECTION_CLOSED`).

Correcao aplicada no Kali (`/etc/hosts`):
```
100.87.201.41 aval.tail5e5757.ts.net
fd7a:115c:a1e0::1c01:c9c4 aval.tail5e5757.ts.net
```
Alternativa: `sudo tailscale set --accept-dns=true` no computador cliente.

## DNS da casa (Pi-hole)

Para a casa toda usar o Pi-hole, configure no roteador (`192.168.1.1`), em DHCP/DNS:
- DNS primario: `192.168.1.18`
- DNS secundario: `1.1.1.1` (se o servidor cair, a internet continua funcionando)

Status em 14/11/2026: **ainda nao configurado no roteador** (dono viajando).
