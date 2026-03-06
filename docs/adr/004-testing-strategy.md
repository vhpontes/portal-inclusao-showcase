# ADR-004: Estratégia de Testes Automatizados (Vitest & Playwright)

## Status
Aceito

## Data
03 de Março de 2026

## Contexto
Com a modernização do frontend para Vue.js 3 e a necessidade de garantir a estabilidade das regras de negócio complexas (como o Wizard de Cadastro e a Portabilidade LGPD), a estratégia de testes precisava ser atualizada para padrões modernos de 2025/2026.

## Decisão
Adotar uma abordagem de testes em duas camadas principais:

1. **Testes Unitários (Vitest)**:
   - Foco em utilitários, composables e lógica isolada de componentes.
   - Execução ultra-rápida via Vite.
   - Localização: `frontend/src/**/__tests__/*.spec.js`.

2. **Testes End-to-End (Playwright)**:
   - Foco em fluxos críticos do usuário (Login, Cadastro de Beneficiário, Pedido de Carteirinha).
   - Testes em navegadores reais (Chromium, Firefox, WebKit).
   - Configuração de bypass para SSL local e suporte a HTTPS.
   - Localização: `frontend/e2e/*.spec.js`.

## Consequências
- **Positivas**: Maior confiança em refatorações, detecção precoce de bugs de interface, documentação viva dos fluxos de negócio.
- **Negativas**: Leve aumento no tempo de build do CI, necessidade de manter ambiente local (backend) ativo para os testes E2E.
