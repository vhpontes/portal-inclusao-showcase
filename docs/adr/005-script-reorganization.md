# ADR-005: Reorganização de Scripts e Inicialização Smart

## Status
Aceito

## Data
03 de Março de 2026

## Contexto
O projeto possuía múltiplos arquivos `.bat` na raiz, dificultando a manutenção e a organização do repositório. Além disso, a inicialização dependia de processos manuais repetitivos.

## Decisão
Mover todos os scripts de automação para uma pasta centralizada `scripts/shell/` e implementar o conceito de **Smart Start**:

1. **Centralização**: Scripts como `run_backend.bat`, `run_frontend.bat` e `start_all.bat` agora residem em `scripts/shell/`.
2. **PowerShell First**: Incentivar o uso de `.ps1` para automações mais complexas (como `smart_start.ps1`), mantendo `.bat` apenas como wrappers de conveniência.
3. **Smart Start**: Um único comando que verifica dependências (Node, PHP, MySQL), instala o que falta e inicia todos os serviços em paralelo com logs coloridos.

## Consequências
- **Positivas**: Raiz do projeto limpa, setup mais rápido para novos desenvolvedores, melhor gerenciamento de serviços em segundo plano.
- **Negativas**: Desenvolvedores precisam se acostumar com a nova localização dos comandos (ex: `.\scripts\shell\smart_start.bat`).
