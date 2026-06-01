# Strategy

## Current Status

Ferramenta totalmente implementada como app Next.js local com todas as funcionalidades core:
- Pipeline: scrape, filtro, ranking, análise com Gemini, geração de conceitos com Claude
- UI: Dashboard, Videos, Run, Configs CRUD, Creators CRUD
- Storage: CSV local em `data/`

## Estratégia para Tiago Zamboni

**Objetivo:** Gerar Reels virais em português para o público brasileiro interessado em imóveis em Orlando.

**Lógica dual de scraping:**
1. Raspar `orlando-real-estate` → capturar temas e mercado local de Orlando
2. Raspar `global-real-estate-viral` → capturar formatos e ganchos virais do mercado imobiliário mundial
3. Misturar: usar tema local com formato viral = vídeo com potencial de alcance + relevância local

**Configs ativas:**
- `TIAGO-ZAMBONI` — analisa criadores de Orlando, gera conceitos adaptados para o público brasileiro
- `TIAGO-ZAMBONI-GLOBAL` — analisa referências virais globais, adapta formato para contexto Orlando/BR

## Next Steps

- Rodar pipeline end-to-end com os creators configurados
- Validar que os conceitos gerados pelo Claude estão no tom certo (português, informal, consultivo)
- Refinar prompts de análise e geração com base nos primeiros resultados
