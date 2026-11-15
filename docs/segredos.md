# Onde ficam os segredos

**Este repositorio nao contem nenhum segredo.** Abaixo, so a localizacao. Todos os `.env` devem ter `chmod 600`.

| Segredo | Arquivo no servidor | Ver / trocar |
|---|---|---|
| Token do bot de noticias + chat autorizado | `~/radar-news-bot/.env` | `/revoke` no @BotFather e editar `TELEGRAM_TOKEN` |
| Senha do banco do Immich | `~/immich-app/.env` (`DB_PASSWORD`) | Trocar exige migrar o Postgres: nao mexer sem necessidade |
| Login do Immich | Dentro do app | Pelo proprio Immich |
| Senhas do Nextcloud (banco e admin inicial) | `~/nextcloud/.env` | Admin: `docker exec -u 33 nextcloud php occ user:resetpassword admin` |
| Senha do painel do Pi-hole | `~/pihole/.env` (`PIHOLE_PW`, valor inicial) | `docker exec pihole pihole setpassword` |
| Senha do Wi-Fi | `/etc/netplan/00-installer-config.yaml` | Trocar no roteador **e** no netplan (antes!), depois `sudo netplan apply` |
| Config do aval-agent | `/etc/aval-agent/config.json` | Ver repo do Bots Central |
| Segredos do Bots Central | No projeto do Bots Central | Ver repo do Bots Central |
| Deploy key do Radar (so leitura) | `~/.ssh/radar_deploy` | GitHub -> repo -> Settings -> Deploy keys |

Ver um segredo:
```bash
ssh -o BatchMode=no voide@100.87.201.41 'cat ~/nextcloud/.env'
```

## Segredos que ja apareceram em conversa (trocar)

Em 14/11/2026 alguns valores foram exibidos em chat durante a instalacao. Recomendado trocar:
- [ ] Token do bot de noticias
- [ ] Senha do painel do Pi-hole
- [ ] Senha do admin do Nextcloud
- [ ] Senha do Wi-Fi (apareceu numa saida do netplan)
