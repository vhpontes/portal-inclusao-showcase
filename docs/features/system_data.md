# Recurso: Gerenciamento de Dados do Sistema

## DescriÃ§Ã£o
Painel administrativo para manutenÃ§Ã£o preventiva e corretiva do banco de dados principal do sistema. Permite visualizaÃ§Ã£o em tempo real da volumetria de dados e execuÃ§Ã£o de rotinas de limpeza e reparo.

## Funcionalidades
1. ðŸ“Š **EstatÃ­sticas de Volumetria**: ExibiÃ§Ã£o da contagem de registros em cada tabela do sistema.
2. ðŸ”„ **Reparo e SeguranÃ§a (Fix-DB)**: Script automatizado que verifica a integridade das tabelas e recria estruturas de seguranÃ§a se necessÃ¡rio.
3. ðŸ§¹ **OtimizaÃ§Ã£o**: Executa `OPTIMIZE TABLE` para liberar espaÃ§o em disco e melhorar performance de busca.
4. ðŸ—‘ï¸ **Limpeza (Truncate)**: Permite limpar dados de tabelas especÃ­ficas (usado em ambiente de staging/teste).
5. ðŸ’¾ **Backup Completo**: BotÃ£o dedicado para exportar o dump completo do banco do sistema.

## CritÃ©rios de AceitaÃ§Ã£o
- [ ] Todas as aÃ§Ãµes destrutivas (Limpar, Reparar) devem exibir um modal de confirmaÃ§Ã£o.
- [ ] O backup deve gerar um arquivo `.sql` compatÃ­vel com MySQL 5.7+.
- [ ] A visualizaÃ§Ã£o de dados das tabelas do sistema deve seguir o mesmo padrÃ£o de performance da importaÃ§Ã£o legada.

## SeguranÃ§a
- Funcionalidade restrita ao nÃ­vel de permissÃ£o mais elevado.
- NotificaÃ§Ãµes de erro detalhadas para falhas de conexÃ£o ou integridade referencial.

