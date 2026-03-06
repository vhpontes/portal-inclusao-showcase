# Recurso: Auditoria LGPD

## Descrição
Módulo dedicado ao rastreamento e monitoramento de ações realizadas no sistema que envolvam dados pessoais ou sensíveis, garantindo a conformidade com a **Lei Geral de Proteção de Dados (LGPD)**.

## Funcionalidades
1. 🛡️ **Rastreamento de Visualização**: Registra sempre que um dado sensível (CPF, Laudo) é exibido na tela.
2. 📤 **Rastreamento de Exportação**: Loga downloads de PDFs, planilhas ou backups.
3. 🔑 **Segurança de Acesso**: Monitora logins bem-sucedidos e tentativas falhas.
4. 📋 **Interface de Consulta**: Visualização cronológica dos logs para administradores.

## Dados Capturados
- **Data/Hora**: Timestamp preciso da ação.
- **Usuário**: Nome e ID do operador (ou "Sistema" para rotinas automáticas).
- **Ação**: Categoria da operação (ex: LOGIN, EXPORTACAO_PDF, VISUALIZACAO_BENEFICIARIO).
- **Detalhes**: Descrição textual do que foi alterado ou acessado.
- **IP Address**: Endereço de origem da requisição.

## Critérios de Aceitação
- [ ] Logs não podem ser editados ou excluídos pela interface.
- [ ] Ações críticas (como exportação de base total) devem ter destaque visual (vermelho).
- [ ] O sistema deve carregar os últimos 100 logs por padrão para agilidade.
