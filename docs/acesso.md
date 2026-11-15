# Acesso ao servidor

## Pela internet (de qualquer lugar)

O acesso remoto e feito **pelo Tailscale**. O SSH nao fica exposto na internet.

1. O computador de onde voce vai acessar precisa estar com o Tailscale ligado e logado na mesma conta.
   ```bash
   sudo systemctl enable --now tailscaled
   sudo tailscale up
   tailscale status | grep aval     # tem que aparecer "active"
   tailscale ping 100.87.201.41
   ```
2. Entrar:
   ```bash
   ssh -o BatchMode=no voide@100.87.201.41
   ```

## Na rede de casa

```bash
ssh voide@192.168.1.18
```

## Atalho recomendado no `~/.ssh/config` do seu computador

```
Host servidor
  HostName 100.87.201.41
  User voide
  IdentityFile ~/.ssh/servidor_aval
  IdentitiesOnly yes
```

Para criar a chave e parar de digitar senha:
```bash
ssh-keygen -t ed25519 -f ~/.ssh/servidor_aval -C "voide@kali"
ssh-copy-id -o BatchMode=no -i ~/.ssh/servidor_aval.pub voide@100.87.201.41
ssh servidor
```

## Cuidado: entrada antiga com `BatchMode yes`

No Kali existe uma entrada `Host 100.87.201.41` com `BatchMode yes` e a chave `kali_bunker_agent` (feita para automacao do Kali Bunker). Ela **impede o SSH de pedir senha** quando voce digita o IP direto. Por isso os comandos desta doc usam `-o BatchMode=no`, ou use o atalho `ssh servidor`.

## Rodar comando remoto sem entrar

```bash
ssh -o BatchMode=no voide@100.87.201.41 'comando aqui'
# se o comando usa sudo, precisa de -t:
ssh -t -o BatchMode=no voide@100.87.201.41 'sudo comando aqui'
```

## Enviar arquivo para o servidor

O `scp` falhou silenciosamente nos testes. Use o proprio SSH:
```bash
ssh -o BatchMode=no voide@100.87.201.41 'cat > ~/arquivo.tar.gz' < arquivo.tar.gz
```

## Acesso fisico

Monitor e teclado direto no OptiPlex. Usuario `voide`. Necessario se o Tailscale e a rede cairem ao mesmo tempo.
