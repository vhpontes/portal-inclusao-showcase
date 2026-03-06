# Recurso: Gerenciamento de Dados do Sistema

## Descrição
Painel administrativo para manutenção preventiva e corretiva do banco de dados principal do sistema. Permite visualização em tempo real da volumetria de dados e execução de rotinas de limpeza e reparo.

## Funcionalidades
1. 📊 **Estatísticas de Volumetria**: Exibição da contagem de registros em cada tabela do sistema.
2. 🔄 **Reparo e Segurança (Fix-DB)**: Script automatizado que verifica a integridade das tabelas e recria estruturas de segurança se necessário.
3. 🧹 **Otimização**: Executa `OPTIMIZE TABLE` para liberar espaço em disco e melhorar performance de busca.
4. 🗑️ **Limpeza (Truncate)**: Permite limpar dados de tabelas específicas (usado em ambiente de staging/teste).
5. 💾 **Backup Completo**: Botão dedicado para exportar o dump completo do banco do sistema.

## Critérios de Aceitação
- [ ] Todas as ações destrutivas (Limpar, Reparar) devem exibir um modal de confirmação.
- [ ] O backup deve gerar um arquivo `.sql` compatível com MySQL 5.7+.
- [ ] A visualização de dados das tabelas do sistema deve seguir o mesmo padrão de performance da importação legada.

## Segurança
- Funcionalidade restrita ao nível de permissão mais elevado.
- Notificações de erro detalhadas para falhas de conexão ou integridade referencial.
