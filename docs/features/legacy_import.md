# Recurso: Importação de Dados Legados

## Descrição
O módulo de **Importação Legada** permite que administradores carreguem dumps SQL de sistemas anteriores diretamente no banco de dados do Portal Inclusão. O sistema processa o arquivo, prefixando automaticamente todas as tabelas com `legado_` para evitar conflitos com as tabelas principais.

## Objetivos
- Facilitar a migração de dados de sistemas legados.
- Permitir a exploração e auditoria de dados antigos antes da transformação final.
- Garantir a separação clara entre dados históricos e dados do novo sistema.

## Funcionalidades
1. 📥 **Upload de SQL**: Suporte a arquivos `.sql` com prefixação automática de tabelas.
2. 🔍 **Explorador de Dados**: Grade interativa para visualizar registros das tabelas `legado_`.
3. 🔢 **Paginador**: Navegação eficiente em tabelas com milhares de registros.
4. ⚙️ **Gerenciamento Granular**:
   - **Resetar Index**: Reinicia o auto-incremento da tabela.
   - **Limpar Dados**: Truncate na tabela mantendo a estrutura.
   - **Excluir Tabela**: Drop table definitivo.
5. 📂 **Backup**: Exportação de tabelas legadas específicas de volta para SQL.

## Critérios de Aceitação
- [ ] O upload deve rejeitar arquivos que não sejam `.sql`.
- [ ] Todas as tabelas criadas devem obrigatoriamente começar com `legado_`.
- [ ] A limpeza total de tabelas legadas deve exigir confirmação em modal de perigo.
- [ ] A visualização de dados deve limitar a 50 registros por página para performance.

## Segurança e Auditoria
- Apenas usuários com perfil `ADMIN` podem acessar esta ferramenta.
- Todas as operações de Drop ou Truncate são registradas nos logs de auditoria do sistema.
