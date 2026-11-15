# Nextcloud (arquivos)

Nuvem pessoal para arquivos e documentos. Versao instalada: 34.0.6 (imagem `nextcloud:stable-apache`).

| Item | Valor |
|---|---|
| Endereco | `https://aval.tail5e5757.ts.net:10000` (**internet**, via Funnel) |
| Usuario admin | `admin` |
| Pasta | `~/nextcloud` |
| Arquivos | `/mnt/storage/nextcloud/data` (HD, dono www-data uid 33) |
| App/config | `~/nextcloud/html` (NVMe) |
| Banco | PostgreSQL 17 em `~/nextcloud/db` |
| Porta local | `127.0.0.1:8095` |
| Tarefas | container `nextcloud-cron` (modo cron) |

## Containers

`nextcloud`, `nextcloud-db`, `nextcloud-redis` (Valkey), `nextcloud-cron`.

## Seguranca (fazer apos instalar)

- Trocar a senha inicial do `admin` (a gerada ficou registrada fora do servidor).
- Ativar 2 fatores: Aplicativos -> "Two-Factor TOTP Provider" -> configurar em Configuracoes pessoais -> Seguranca.

## Comandos `occ`

```bash
docker exec -u 33 nextcloud php occ status
docker exec -u 33 nextcloud php occ user:list
docker exec -u 33 nextcloud php occ user:resetpassword admin
docker exec -u 33 nextcloud php occ maintenance:mode --on   # / --off
docker exec -u 33 nextcloud php occ files:scan --all        # apos copiar arquivos direto no HD
```

## Operacao

```bash
cd ~/nextcloud
docker compose ps
docker compose logs --tail 50 app

# atualizar: uma versao principal por vez
docker compose pull && docker compose up -d
```

## Se mudar a porta/endereco

```bash
docker exec -u 33 nextcloud php occ config:system:set overwrite.cli.url --value=https://aval.tail5e5757.ts.net:PORTA
docker exec -u 33 nextcloud php occ config:system:get trusted_domains
```
