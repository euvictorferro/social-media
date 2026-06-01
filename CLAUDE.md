# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

## What This Is

**Social Media AI** — a tool that helps create viral Instagram Reels by analyzing competitor content. It scrapes competitors' recent videos, identifies the most viral ones, analyzes them with AI (video understanding + content breakdown), and generates new adapted video concepts for a given brand.

---

## Clientes Ativos

### Tiago Zamboni (`@tiagohzamboni`)
Corretor imobiliário brasileiro em Orlando, FL.
Contexto completo: `context/tiago-zamboni.md`

**Estratégia de creators:**
- `orlando-real-estate` → criadores locais de Orlando (temas e mercado local)
- `global-real-estate-viral` → referências virais globais de imóveis (formatos e ganchos)

**Configs ativas:**
- `TIAGO-ZAMBONI` — raspa criadores de Orlando, gera conceitos em português para o público brasileiro
- `TIAGO-ZAMBONI-GLOBAL` — raspa referências virais globais, adapta formato para o contexto Orlando/brasileiro

---

### Laís Daltrozo (`@laisdaltrozo`)
Especialista em life insurance e proteção patrimonial nos EUA.
Contexto completo: `context/lais-daltrozo.md`

**Estratégia de creators:**
- `lais-insurance-niche` → referências do nicho (life insurance + jurídico)
- `lais-viral-formats` → referências de formato viral adaptáveis ao nicho dela

**Configs ativas:**
- `LAIS-DALTROZO` — raspa creators do nicho, gera conceitos em português para brasileiros nos EUA
- `LAIS-VIRAL-FORMATS` — raspa referências de formato viral, adapta gancho/estrutura para o contexto de life insurance

---

## How to Run

```bash
cd app
npm install
npm run dev
# Open http://localhost:3000
```

**Required environment variables** (in `.env` at project root):
- `APIFY_API_TOKEN` — Apify Instagram scraper
- `GEMINI_API_KEY` — Google Gemini video analysis
- `ANTHROPIC_API_KEY` — Claude concept generation

---

## Tech Stack

- **Next.js 16** (App Router) + **TypeScript**
- **Tailwind CSS** + **shadcn/ui** components
- **CSV files** for data storage (in `data/` directory)
- **Apify** — Instagram scraping
- **Google Gemini 2.0 Flash** — Video analysis (upload + multimodal)
- **Claude Sonnet** — New concept generation

---

## How The System Works

### Pipeline Overview

1. **Input** — Select a config and parameters (max videos, top-K, days lookback) via the Run page
2. **Load Config** — Retrieve analysis prompt, new concepts prompt, and creator list from CSV
3. **Scrape** — For each competitor creator, scrape recent Instagram Reels via Apify
4. **Filter & Rank** — Filter by date, sort by views, take top-K most viral
5. **Analyze** — Download video, upload to Gemini, analyze (extracts Concept, Hook, Retention, Reward, Script)
6. **Generate** — Send analysis + brand context to Claude for adapted video concepts
7. **Save** — Append results to `data/videos.csv`, viewable in the Videos page with thumbnails

### Two Customizable Prompts Per Config

- **Analysis Instruction** — How Gemini should break down the video
- **New Concepts Instruction** — How Claude should adapt the reference for the brand

---

## Workspace Structure

```
.
├── CLAUDE.md                              # This file
├── .env                                   # API keys (not committed)
├── app/                                   # Next.js application
│   ├── src/
│   │   ├── app/                           # Pages and API routes
│   │   │   ├── page.tsx                   # Dashboard
│   │   │   ├── videos/page.tsx            # Videos browser with thumbnails
│   │   │   ├── run/page.tsx               # Pipeline runner with live progress
│   │   │   ├── configs/page.tsx           # Config management
│   │   │   ├── creators/page.tsx          # Creator management
│   │   │   └── api/                       # API routes (configs, creators, videos, pipeline)
│   │   ├── lib/                           # Core logic
│   │   │   ├── pipeline.ts               # Pipeline orchestration
│   │   │   ├── apify.ts                  # Apify scraper client
│   │   │   ├── gemini.ts                 # Gemini video analysis client
│   │   │   ├── claude.ts                 # Claude concept generation client
│   │   │   ├── csv.ts                    # CSV read/write utilities
│   │   │   └── types.ts                  # TypeScript interfaces
│   │   └── components/                    # UI components (shadcn + custom)
│   └── package.json
├── data/                                  # CSV data storage
│   ├── configs.csv                        # Pipeline configurations
│   ├── creators.csv                       # Instagram creator accounts
│   └── videos.csv                         # Analyzed video results
├── context/                               # Background context for Claude
├── plans/                                 # Implementation plans
└── .claude/commands/                      # Slash commands (prime, create-plan, implement)
```

---

## App Pages

| Page | Path | Description |
|------|------|-------------|
| Dashboard | `/` | Summary stats, recent videos |
| Videos | `/videos` | Browse results with thumbnails, expandable analysis & concepts |
| Run Pipeline | `/run` | Select config, set params, run with live progress streaming |
| Configs | `/configs` | CRUD for pipeline configs (prompts, categories) |
| Creators | `/creators` | CRUD for competitor Instagram accounts |

---

## Commands

### /prime
Initialize a new session with full context awareness.

### /create-plan [request]
Create a detailed implementation plan in `plans/`.

### /implement [plan-path]
Execute a plan step by step.

---

## Regras Estratégicas de Conteúdo (Clique Boost)

Essas regras se aplicam a qualquer planejamento de conteúdo feito neste sistema para clientes da Clique Boost.

### Stories são complementares — nunca substitutos

Stories não ocupam o lugar de um post. Eles existem ao mesmo tempo que Reels, carrosséis e TikToks, funcionando como camada de relacionamento e conversão por cima do conteúdo principal.

**Regra:** se um dia tem Reel, também tem Stories. Se tem carrossel, também tem Stories. Nunca planejar Stories como "o conteúdo do dia" em substituição a um post de feed.

Função de cada formato:
- **Reel / TikTok** → alcance e descoberta (topo de funil)
- **Carrossel** → educação e salvamentos (meio de funil)
- **Stories** → relacionamento, bastidores e conversão (fundo de funil)

Os três funcionam juntos no mesmo dia. Separar por dia quebra o funil.

### Reels e TikTok são o mesmo conteúdo — publicar em paralelo

Todo Reel gravado vai para o TikTok também, com no máximo 2h de diferença entre as publicações. O TikTok favorece conteúdo novo — nunca postar o mesmo vídeo com mais de 24h de intervalo entre plataformas.

**Regra:** ao planejar um Reel, já incluir TikTok no mesmo card/dia. Nunca tratar como formatos separados no calendário.

### Volume mínimo por semana

- Feed (Reels + carrosséis): 4–5 posts
- Stories: todos os dias (mínimo 1 slide diário)
- TikTok: espelha todos os Reels do Instagram

### Apify — descoberta de contas

O Apify funciona com usernames exatos. Para descobrir novos players de um nicho, usar o hashtag scraper primeiro para mapear usernames e depois raspar os perfis individualmente. Nunca tentar adivinhar usernames sem validação prévia.

---

## Critical Instruction: Maintain This File

After any change to the workspace, ask:
1. Does this change add new functionality?
2. Does it modify the workspace structure documented above?
3. Should a new command be listed?
4. Does context/ need updates?

If yes, update the relevant sections.

---

## Session Workflow

1. **Start**: Run `/prime` to load context
2. **Work**: Use commands or direct Claude with tasks
3. **Plan changes**: Use `/create-plan` before significant additions
4. **Execute**: Use `/implement` to execute plans
5. **Maintain**: Claude updates CLAUDE.md and context/ as the workspace evolves
