# Bots Central V2 e aval-agent

Projeto principal do servidor. Repositorio proprio: `github.com/Voide-Kali/Bots-Central-V2`. **A documentacao detalhada fica la.** Aqui so o que importa para operar o servidor.

> Regra: o Bots Central e o `aval-agent` nao podem cair. Nao altere o compose do Bots Central a partir de outros projetos.

## Containers

| Container | Funcao |
|---|---|
| `bots-central-web-1` | Frontend/proxy (Caddy). Publicado em `127.0.0.1:8082` |
| `bots-central-backend-1` | API (Python/FastAPI) |
| `bots-central-telegram-1` | Bot do Telegram do Bots Central |
| `bots-central-db-1` | PostgreSQL 17 |
| `bots-central-backup-1` | Backups para `/mnt/storage/bots-central-backups` |
| `bots-central-downloads-1` | Caddy para downloads |
| `bots-central-ollama-1` | Ollama do Bots Central (modelo `qwen2.5:3b`), sem porta publicada |

Redes: `bots-central_internal` e `bots-central_egress`. Todos com `restart: unless-stopped`.

Acesso publico: `https://aval.tail5e5757.ts.net` (Funnel 443 -> `127.0.0.1:8082`).

## aval-agent

Servico systemd (nao e Docker):
```
/etc/systemd/system/aval-agent.service
ExecStart=/usr/local/bin/aval-agent -config /etc/aval-agent/config.json
Restart=on-failure, RestartSec=10
```
Comandos:
```bash
systemctl status aval-agent --no-pager
journalctl -u aval-agent -n 50 --no-pager
```

## Verificar saude

```bash
docker ps --filter name=bots-central --format '{{.Names}}\t{{.Status}}'
systemctl is-active aval-agent
```
