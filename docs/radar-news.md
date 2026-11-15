# Radar News Bot

Bot pessoal de noticias no Telegram (`@radar_voide_bot`). Coleta ~36 fontes RSS, traduz e resume as em ingles com IA local e manda em portugues.

- Repositorio: `github.com/Voide-Kali/radar-news-bot` (privado). Plano completo em `PLANO.md` la.
- Pasta no servidor: `~/radar-news-bot`
- Branch em producao (14/11/2026): `phase/f2-f5-bot` (ainda nao mesclada na `main`)

## Containers (projeto compose `radar`)

| Container | Funcao |
|---|---|
| `radar-bot` | Bot (Python 3.13, usuario 1000, filesystem somente leitura, sem capabilities) |
| `radar-ollama` | Ollama proprio com `qwen2.5:3b`, limitado a 3 CPUs e 6 GB, **sem porta publicada** |

Rede propria `radar_radar`. Nao compartilha nada com o Bots Central.

## Como funciona

- Coleta a cada 15 min. Categorias: Ciber e Regional chegam na hora; Tech, Lancamentos, Mundo e Brasil em resumos as 7h30, 12h30, 19h30; Mercado as 18h30 (dias uteis).
- Silencio noturno 23h30-6h30. Noticias `[CRITICO]` furam pausa e silencio.
- Fontes e horarios: `~/radar-news-bot/feeds.yaml` (editavel sem mexer no codigo).
- Banco: `~/radar-news-bot/data/radar.db` (SQLite).

## Telegram

Teclado: `Pausar / Ativar / Status / Categorias / Agora`. Comandos: `/status`, `/agora`, `/pausar`, `/ativar`, `/categorias`, `/resumo <categoria>`.

So o chat em `ALLOWED_CHAT_IDS` (no `.env`) e atendido; outros sao ignorados.

## Operacao

```bash
cd ~/radar-news-bot
docker compose ps
docker compose logs -f --tail 50 radar-bot
docker compose restart radar-bot

# atualizar depois de um push no GitHub
git pull && docker compose up -d --build

# testar fontes (fora do Docker, usa o .venv)
.venv/bin/python -m newsbot check-feeds
```

O servidor puxa do GitHub com uma **deploy key somente leitura** (`~/.ssh/radar_deploy`, configurada em `git config core.sshCommand`).

## Trocar o token do bot

1. No @BotFather: `/revoke` e escolha o bot. Copie o token novo.
2. No servidor:
   ```bash
   cd ~/radar-news-bot && nano .env    # TELEGRAM_TOKEN=...
   docker compose up -d
   ```
