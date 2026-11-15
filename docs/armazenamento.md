# Armazenamento

## Discos

| Disco | Tamanho | Montagem | Uso |
|---|---|---|---|
| NVMe KIOXIA (`nvme0n1`) | 256 GB | `/` (LVM `ubuntu-vg/ubuntu-lv`), `/boot`, `/boot/efi` | Sistema, Docker, bancos de dados |
| HD WD10JPVX (`sda2`, label `STORJ_DATA`) | 1 TB | `/mnt/storage` | Fotos, arquivos, backups |

O HD monta sozinho no boot pelo `/etc/fstab`:
```
UUID=0ea869ef-514f-4983-a09a-b8d666c69405 /mnt/storage ext4 defaults,noatime,nofail,x-systemd.device-timeout=10 0 2
```
`nofail` = se o HD nao estiver conectado, o servidor liga mesmo assim (mas os servicos que dependem dele ficam sem dados).

## O que tem em `/mnt/storage`

| Pasta | Dono | Conteudo |
|---|---|---|
| `immich/library/` | voide (1000) | Fotos e videos do Immich |
| `nextcloud/data/` | www-data (33) | Arquivos do Nextcloud |
| `bots-central-backups/` | root | Backups automaticos do Bots Central |
| `storj/` | voide:docker | Dados de um no Storj (nao esta rodando) |
| `amplumen-retirement-20260920T.../` | root | Arquivo do projeto Amplumen desativado |
| `lost+found/` | root | Padrao do ext4 |

## O que fica no NVMe (home do voide)

| Pasta | Conteudo |
|---|---|
| `~/radar-news-bot/` | Bot de noticias (codigo, `.env`, `data/radar.db`) |
| `~/immich-app/` | Compose e `.env` do Immich, banco em `postgres/` |
| `~/nextcloud/` | Compose e `.env` do Nextcloud, app em `html/`, banco em `db/` |
| `~/pihole/` | Compose e `.env` do Pi-hole, config em `etc-pihole/` |
| `~/adguard/` | AdGuard Home antigo (parado) |
| `~/Bots-Central-V2*` | Repositorios/worktrees do Bots Central |

Volumes Docker nomeados (em `/var/lib/docker/volumes`):
- `radar_radar-ollama-models`: modelo de IA do bot de noticias (~2 GB)
- `immich_model-cache`: modelos de IA do Immich

## ATENCAO: nao existe backup fora deste HD

Fotos, arquivos e os backups do Bots Central estao **todos no mesmo HD**. Se ele falhar, tudo se perde junto. Ver [pendencias.md](pendencias.md).

Ver uso:
```bash
df -h / /mnt/storage
sudo du -sh /mnt/storage/* 2>/dev/null
docker system df
```
