# ADR-004: EstratÃ©gia de Testes Automatizados (Vitest & Playwright)

## Status
Aceito

## Data
03 de MarÃ§o de 2026

## Contexto
Com a modernizaÃ§Ã£o do frontend para Vue.js 3 e a necessidade de garantir a estabilidade das regras de negÃ³cio complexas (como o Wizard de Cadastro e a Portabilidade LGPD), a estratÃ©gia de testes precisava ser atualizada para padrÃµes modernos de 2025/2026.

## DecisÃ£o
Adotar uma abordagem de testes em duas camadas principais:

1. **Testes UnitÃ¡rios (Vitest)**:
   - Foco em utilitÃ¡rios, composables e lÃ³gica isolada de componentes.
   - ExecuÃ§Ã£o ultra-rÃ¡pida via Vite.
   - LocalizaÃ§Ã£o: `frontend/src/**/__tests__/*.spec.js`.

2. **Testes End-to-End (Playwright)**:
   - Foco em fluxos crÃ­ticos do usuÃ¡rio (Login, Cadastro de BeneficiÃ¡rio, Pedido de Carteirinha).
   - Testes em navegadores reais (Chromium, Firefox, WebKit).
   - ConfiguraÃ§Ã£o de bypass para SSL local e suporte a HTTPS.
   - LocalizaÃ§Ã£o: `frontend/e2e/*.spec.js`.

## ConsequÃªncias
- **Positivas**: Maior confianÃ§a em refatoraÃ§Ãµes, detecÃ§Ã£o precoce de bugs de interface, documentaÃ§Ã£o viva dos fluxos de negÃ³cio.
- **Negativas**: Leve aumento no tempo de build do CI, necessidade de manter ambiente local (backend) ativo para os testes E2E.

