# Outros itens no servidor

| Item | Estado | Observacao |
|---|---|---|
| AdGuard Home | Container `adguardhome` **parado** (Exited) desde ~set/2026 | Pasta `~/adguard`. Faz o mesmo que o Pi-hole: nao ligar os dois juntos (porta 53) |
| Storj | Pasta `/mnt/storage/storj`, nenhum container rodando | Restos de um no Storj |
| libvirt / QEMU | Ativo, nenhuma VM criada | Rede de VMs `192.168.122.0/24` (dnsmasq) |
| xrdp | Ativo | Area de trabalho remota (porta 3389). Desativar se nao usar: `sudo systemctl disable --now xrdp xrdp-sesman` |
| LogMeIn Hamachi | Ativo e conectado a um peer | VPN antiga. Nao mexido |
| Ambiente grafico (GNOME/snap firefox) | Instalado | Ubuntu desktop |
| kdump-tools | Ativo | Captura de crash do kernel |
| unattended-upgrades | Ativo | Atualizacoes automaticas de seguranca |

Pastas na home com outros projetos: `.codex-work/`, `amplumen-data/`, `backups/` (sem permissao para o usuario).
