# Recurso: Privacidade e Consentimento (Self-Service LGPD)

## Descrição
Portal dedicado ao cidadão, permitindo que o titular dos dados exerça seus direitos previstos no Art. 18 da LGPD de forma autônoma e transparente.

## Funcionalidades
1. 📜 **Visualização da Política**: Página interativa e navegável com a última versão da Política de Privacidade.
2. ✅ **Gestão de Consentimentos**: Lista de todos os termos aceitos pelo usuário, com data, IP e versão da política.
3. ❌ **Revogação de Consentimento**: Botão para solicitar a interrupção do tratamento de dados para finalidades específicas.
4. 📂 **Portabilidade de Dados**: Exportação de todos os dados do cidadão (cadastros, laudos, fotos) em um pacote `.zip` estruturado.
5. 🕰️ **Transparência Ativa (Meus Logs)**: Histórico de quando e como o sistema acessou os dados do próprio usuário.

## Fluxo de Portabilidade
1. O usuário clica em "Exportar Meus Dados".
2. O servidor gera um arquivo ZIP contendo:
   - Arquivo JSON com dados cadastrais.
   - Pasta `docs/` com cópias dos laudos enviados.
   - Foto do perfil.
3. O download é iniciado automaticamente.
4. Uma entrada é gerada no log de auditoria global registrando a exportação.

## Critérios de Aceitação
- [ ] A revogação de consentimentos essenciais deve exibir um alerta sobre a impossibilidade de continuar usando o serviço.
- [ ] O pacote de portabilidade deve vir criptografado ou em canal seguro HTTPS.
- [ ] A página de política de privacidade deve ser acessível mesmo sem login.
