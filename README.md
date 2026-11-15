# Servidor tropa (aval)

Documentacao do servidor de casa: o que roda nele, onde fica cada coisa e como mexer sem quebrar nada.

> Nenhuma senha, token ou chave esta neste repositorio. Os segredos ficam nos arquivos `.env` de cada servico no proprio servidor (ver [docs/segredos.md](docs/segredos.md)).

Ultima revisao: 14/11/2026.

## Resumo rapido

| Item | Valor |
|---|---|
| Hostname local | `tropa` |
| Nome no Tailscale | `aval` (`aval.tail5e5757.ts.net`) |
| IP Tailscale | `100.87.201.41` |
| IP na rede de casa | `192.168.1.18` (fixo, Wi-Fi `wlp3s0`) |
| Usuario | `voide` (uid 1000) |
| Hardware | Dell OptiPlex 3080, Intel i3-10100T, 16 GB RAM, sem GPU |
| Disco do sistema | NVMe KIOXIA 256 GB (LVM, ext4) |
| Disco de dados | HD WD 1 TB em `/mnt/storage` |
| Sistema | Ubuntu 26.04 LTS, Docker 29 (live-restore ligado) |

## O que roda nele

| Servico | Para que serve | Endereco | Quem acessa | Doc |
|---|---|---|---|---|
| Bots Central V2 | Plataforma de bots / gerenciamento (projeto principal) | `https://aval.tail5e5757.ts.net` | Internet (Funnel) | [bots-central.md](docs/bots-central.md) |
| aval-agent | Agente de heartbeat/telemetria assinado | - | - | [bots-central.md](docs/bots-central.md) |
| Radar News Bot | Bot de noticias no Telegram com IA local | `@radar_voide_bot` | So o chat do dono | [radar-news.md](docs/radar-news.md) |
| Immich | Fotos e videos (alternativa ao Google Fotos) | `https://aval.tail5e5757.ts.net:8443` | Internet (Funnel) | [immich.md](docs/immich.md) |
| Nextcloud | Arquivos e documentos (nuvem pessoal) | `https://aval.tail5e5757.ts.net:10000` | Internet (Funnel) | [nextcloud.md](docs/nextcloud.md) |
| Pi-hole | DNS com bloqueio de anuncios para a casa | DNS `192.168.1.18:53` / painel `https://aval.tail5e5757.ts.net:8445/admin` | Rede de casa / so tailnet | [pihole.md](docs/pihole.md) |

Outros itens presentes (parados ou auxiliares): AdGuard Home (parado), pasta Storj, libvirt/QEMU, xrdp, LogMeIn Hamachi. Ver [outros.md](docs/outros.md).

## Onde procurar

| Preciso de... | Va para |
|---|---|
| Entrar no servidor | [docs/acesso.md](docs/acesso.md) |
| Saber o que esta exposto na internet | [docs/rede-e-exposicao.md](docs/rede-e-exposicao.md) |
| Saber onde ficam os dados | [docs/armazenamento.md](docs/armazenamento.md) |
| Atualizar o sistema sem derrubar nada | [docs/manutencao.md](docs/manutencao.md) |
| Reiniciar o servidor com seguranca | [docs/manutencao.md](docs/manutencao.md#reboot-seguro) |
| Resolver um problema conhecido | [docs/problemas-conhecidos.md](docs/problemas-conhecidos.md) |
| Saber onde esta cada senha | [docs/segredos.md](docs/segredos.md) |
| Ver o que ainda falta fazer | [docs/pendencias.md](docs/pendencias.md) |
| Historico de mudancas | [CHANGELOG.md](CHANGELOG.md) |

## Regras de ouro

1. **O Bots Central e o `aval-agent` nao podem cair.** Qualquer mudanca no servidor e feita de forma que eles continuem rodando.
2. **Cada servico tem sua pasta, seu `docker-compose.yml` e sua rede.** Nenhum servico novo mexe no compose de outro.
3. **Antes e depois de qualquer mudanca**, rode o checklist de saude:
   ```bash
   docker ps --format '{{.Names}}\t{{.Status}}' | sort; systemctl is-active aval-agent
   ```
4. **Segredos nunca vao para o Git.** Ficam em `.env` com `chmod 600`.
5. Exposicao na internet (Funnel) so com senha forte e, quando possivel, 2 fatores.
