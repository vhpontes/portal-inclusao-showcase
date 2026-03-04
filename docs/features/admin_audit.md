# Recurso: Auditoria LGPD

## DescriÃ§Ã£o
MÃ³dulo dedicado ao rastreamento e monitoramento de aÃ§Ãµes realizadas no sistema que envolvam dados pessoais ou sensÃ­veis, garantindo a conformidade com a **Lei Geral de ProteÃ§Ã£o de Dados (LGPD)**.

## Funcionalidades
1. ðŸ›¡ï¸ **Rastreamento de VisualizaÃ§Ã£o**: Registra sempre que um dado sensÃ­vel (CPF, Laudo) Ã© exibido na tela.
2. ðŸ“¤ **Rastreamento de ExportaÃ§Ã£o**: Loga downloads de PDFs, planilhas ou backups.
3. ðŸ”‘ **SeguranÃ§a de Acesso**: Monitora logins bem-sucedidos e tentativas falhas.
4. ðŸ“‹ **Interface de Consulta**: VisualizaÃ§Ã£o cronolÃ³gica dos logs para administradores.

## Dados Capturados
- **Data/Hora**: Timestamp preciso da aÃ§Ã£o.
- **UsuÃ¡rio**: Nome e ID do operador (ou "Sistema" para rotinas automÃ¡ticas).
- **AÃ§Ã£o**: Categoria da operaÃ§Ã£o (ex: LOGIN, EXPORTACAO_PDF, VISUALIZACAO_BENEFICIARIO).
- **Detalhes**: DescriÃ§Ã£o textual do que foi alterado ou acessado.
- **IP Address**: EndereÃ§o de origem da requisiÃ§Ã£o.

## CritÃ©rios de AceitaÃ§Ã£o
- [ ] Logs nÃ£o podem ser editados ou excluÃ­dos pela interface.
- [ ] AÃ§Ãµes crÃ­ticas (como exportaÃ§Ã£o de base total) devem ter destaque visual (vermelho).
- [ ] O sistema deve carregar os Ãºltimos 100 logs por padrÃ£o para agilidade.

