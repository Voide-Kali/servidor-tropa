# Problemas conhecidos e solucoes

## SSH recusa sem pedir senha ("Permission denied (publickey,password)")
Causa: entrada no `~/.ssh/config` do Kali com `BatchMode yes`.
Solucao: `ssh -o BatchMode=no voide@100.87.201.41` ou usar o atalho `ssh servidor` (ver [acesso.md](acesso.md)).

## SSH da "Connection timed out"
Causa: Tailscale desligado no computador cliente.
```bash
sudo systemctl enable --now tailscaled && sudo tailscale up
tailscale ping 100.87.201.41
```

## `scp` volta sem enviar e sem erro
Contorno: `ssh -o BatchMode=no voide@100.87.201.41 'cat > ~/arquivo' < arquivo`

## Navegador da ERR_CONNECTION_CLOSED em `aval.tail5e5757.ts.net:<porta>`
Causa: o nome resolve para o Funnel publico (IPv6 `2607:f740:...`) porque o cliente nao usa MagicDNS. Portas so-tailnet nao funcionam por esse caminho.
Solucao no cliente: adicionar ao `/etc/hosts`
```
100.87.201.41 aval.tail5e5757.ts.net
fd7a:115c:a1e0::1c01:c9c4 aval.tail5e5757.ts.net
```
e limpar o cache do Chrome em `chrome://net-internals/#dns`.

## Painel do Pi-hole (`192.168.1.18`) nao abre
Fora de casa esse IP nao existe. Use `https://aval.tail5e5757.ts.net:8445/admin`.
Nunca configure o DNS do notebook para `192.168.1.18` fora de casa: a internet para.

## Boot demora ~2 min
Corrigido em 14/11/2026 com override do `systemd-networkd-wait-online` (ver [manutencao.md](manutencao.md#boot)).

## Git na home por engano
Um `git init` rodado em `~` cria `~/.git` e tenta versionar a home inteira. Se acontecer: `rm -rf ~/.git` (so se acabou de ser criado).

## Comandos colados com heredoc saem com `\` a mais
Ao colar scripts longos no terminal, o zsh pode inserir `\` antes de `;`. Conferir o script no servidor (`cat ~/script.sh`) antes de rodar.

## Bot de noticias: botoes nao aparecem
Mandar `/start` depois de o chat estar em `ALLOWED_CHAT_IDS`. No app, tocar no icone de teclado ao lado do campo de texto.
