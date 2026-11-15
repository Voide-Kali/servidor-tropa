# Pendencias

## Seguranca
- [ ] Trocar segredos expostos em conversa (ver [segredos.md](segredos.md#segredos-que-ja-apareceram-em-conversa-trocar))
- [ ] Ativar 2 fatores no Nextcloud
- [ ] Decidir se `xrdp` e `logmein-hamachi` continuam (superficie de ataque)
- [ ] Confirmar que o painel do Bots Central precisa mesmo ficar no Funnel (decisao atual: sim)

## Backup (prioridade)
- [ ] Hoje fotos (Immich), arquivos (Nextcloud) e backups do Bots Central estao **no mesmo HD**. Criar uma segunda copia (outro disco, outro computador ou nuvem com criptografia, ex.: restic/borg)
- [ ] Backup dos bancos que ficam no NVMe: `~/immich-app/postgres`, `~/nextcloud/db`, `~/radar-news-bot/data`

## Rede
- [ ] Roteador: DNS primario `192.168.1.18`, secundario `1.1.1.1` (quando voltar para casa)
- [ ] Ligar o servidor por cabo de rede em vez de Wi-Fi
- [ ] Opcional: usar o Pi-hole tambem fora de casa via DNS da tailnet

## Hardware (precisa estar perto do servidor)
- [ ] BIOS Dell: **AC Power Recovery = Power On** (religar sozinho apos queda de energia)
- [ ] Atualizar firmware (`fwupdmgr get-upgrades`)

## Radar News Bot
- [ ] Validar mensagens por uns dias e fazer merge de `phase/f2-f5-bot` na `main`
- [ ] Criar tag `radar-f1-v0.1` no GitHub e apagar a branch `phase/f1-coletor`

## Limpeza
- [ ] Decidir sobre o AdGuard parado (`~/adguard`, container `adguardhome`)
- [ ] Decidir sobre a pasta `/mnt/storage/storj`
- [ ] Remover scripts de instalacao da home: `~/instalar-immich.sh`, `~/instalar-pihole.sh`, `~/instalar-nextcloud.sh`, `~/radar-news-bot-f1*.tar.gz`
