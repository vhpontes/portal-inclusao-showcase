# Recurso: ImportaÃ§Ã£o de Dados Legados

## DescriÃ§Ã£o
O mÃ³dulo de **ImportaÃ§Ã£o Legada** permite que administradores carreguem dumps SQL de sistemas anteriores diretamente no banco de dados do Portal InclusÃ£o. O sistema processa o arquivo, prefixando automaticamente todas as tabelas com `legado_` para evitar conflitos com as tabelas principais.

## Objetivos
- Facilitar a migraÃ§Ã£o de dados de sistemas legados.
- Permitir a exploraÃ§Ã£o e auditoria de dados antigos antes da transformaÃ§Ã£o final.
- Garantir a separaÃ§Ã£o clara entre dados histÃ³ricos e dados do novo sistema.

## Funcionalidades
1. ðŸ“¥ **Upload de SQL**: Suporte a arquivos `.sql` com prefixaÃ§Ã£o automÃ¡tica de tabelas.
2. ðŸ” **Explorador de Dados**: Grade interativa para visualizar registros das tabelas `legado_`.
3. ðŸ”¢ **Paginador**: NavegaÃ§Ã£o eficiente em tabelas com milhares de registros.
4. âš™ï¸ **Gerenciamento Granular**:
   - **Resetar Index**: Reinicia o auto-incremento da tabela.
   - **Limpar Dados**: Truncate na tabela mantendo a estrutura.
   - **Excluir Tabela**: Drop table definitivo.
5. ðŸ“‚ **Backup**: ExportaÃ§Ã£o de tabelas legadas especÃ­ficas de volta para SQL.

## CritÃ©rios de AceitaÃ§Ã£o
- [ ] O upload deve rejeitar arquivos que nÃ£o sejam `.sql`.
- [ ] Todas as tabelas criadas devem obrigatoriamente comeÃ§ar com `legado_`.
- [ ] A limpeza total de tabelas legadas deve exigir confirmaÃ§Ã£o em modal de perigo.
- [ ] A visualizaÃ§Ã£o de dados deve limitar a 50 registros por pÃ¡gina para performance.

## SeguranÃ§a e Auditoria
- Apenas usuÃ¡rios com perfil `ADMIN` podem acessar esta ferramenta.
- Todas as operaÃ§Ãµes de Drop ou Truncate sÃ£o registradas nos logs de auditoria do sistema.

