# Immich (fotos)

Alternativa ao Google Fotos. Versao instalada: v3 (v3.4.1 em 14/11/2026).

| Item | Valor |
|---|---|
| Endereco | `https://aval.tail5e5757.ts.net:8443` (**internet**, via Funnel) |
| Pasta | `~/immich-app` (compose oficial + `docker-compose.override.yml`) |
| Fotos | `/mnt/storage/immich/library` (HD) |
| Banco | `~/immich-app/postgres` (NVMe) |
| Porta local | `127.0.0.1:2283` |
| Limites | servidor 3 CPUs, machine learning 2 CPUs |

## Containers

`immich_server`, `immich_machine_learning`, `immich_postgres`, `immich_redis`.

## Apps

App "Immich" (Android/iOS) -> servidor `https://aval.tail5e5757.ts.net:8443` -> ativar backup automatico.

Contas novas: so o admin cria (Administracao -> Usuarios). Nao existe cadastro aberto.

## Operacao

```bash
cd ~/immich-app
docker compose ps
docker compose logs --tail 50 immich-server

# atualizar (ler as release notes antes: github.com/immich-app/immich/releases)
docker compose pull && docker compose up -d
```

## Voltar a ser so tailnet (fechar da internet)

```bash
sudo tailscale funnel --https=8443 off
sudo tailscale serve --bg --https=8443 http://127.0.0.1:2283
```
