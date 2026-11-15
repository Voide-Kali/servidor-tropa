# Pi-hole (DNS com bloqueio de anuncios)

| Item | Valor |
|---|---|
| DNS | `192.168.1.18:53` (tcp/udp) - so rede de casa |
| Painel (casa) | `http://192.168.1.18:8090/admin` |
| Painel (fora de casa) | `https://aval.tail5e5757.ts.net:8445/admin` (so tailnet) |
| Pasta | `~/pihole` |
| Config | `~/pihole/etc-pihole` |
| DNS de saida | `1.1.1.1` e `9.9.9.9` |
| Limites | 1 CPU, 512 MB |

O Pi-hole escuta **so no IP da rede** (`192.168.1.18`). O DNS do proprio servidor (`systemd-resolved` em `127.0.0.53`) nao foi alterado.

## Ativar para a casa toda

No roteador `192.168.1.1`: DNS primario `192.168.1.18`, secundario `1.1.1.1`. **Pendente** em 14/11/2026.

## Testar

```bash
dig +short @192.168.1.18 google.com        # IP normal
dig +short @192.168.1.18 doubleclick.net   # 0.0.0.0 = bloqueado
```

## Operacao

```bash
cd ~/pihole
docker compose ps
docker compose logs --tail 50
docker compose pull && docker compose up -d     # atualizar
docker exec pihole pihole -g                   # atualizar listas de bloqueio
docker exec pihole pihole setpassword          # trocar senha do painel
```

Observacao: o script de instalacao **gera uma senha nova a cada execucao**. Nao rode `~/instalar-pihole.sh` de novo; use `pihole setpassword`.
