# ADR-005: ReorganizaÃ§Ã£o de Scripts e InicializaÃ§Ã£o Smart

## Status
Aceito

## Data
03 de MarÃ§o de 2026

## Contexto
O projeto possuÃ­a mÃºltiplos arquivos `.bat` na raiz, dificultando a manutenÃ§Ã£o e a organizaÃ§Ã£o do repositÃ³rio. AlÃ©m disso, a inicializaÃ§Ã£o dependia de processos manuais repetitivos.

## DecisÃ£o
Mover todos os scripts de automaÃ§Ã£o para uma pasta centralizada `scripts/shell/` e implementar o conceito de **Smart Start**:

1. **CentralizaÃ§Ã£o**: Scripts como `run_backend.bat`, `run_frontend.bat` e `start_all.bat` agora residem em `scripts/shell/`.
2. **PowerShell First**: Incentivar o uso de `.ps1` para automaÃ§Ãµes mais complexas (como `smart_start.ps1`), mantendo `.bat` apenas como wrappers de conveniÃªncia.
3. **Smart Start**: Um Ãºnico comando que verifica dependÃªncias (Node, PHP, MySQL), instala o que falta e inicia todos os serviÃ§os em paralelo com logs coloridos.

## ConsequÃªncias
- **Positivas**: Raiz do projeto limpa, setup mais rÃ¡pido para novos desenvolvedores, melhor gerenciamento de serviÃ§os em segundo plano.
- **Negativas**: Desenvolvedores precisam se acostumar com a nova localizaÃ§Ã£o dos comandos (ex: `.\scripts\shell\smart_start.bat`).

