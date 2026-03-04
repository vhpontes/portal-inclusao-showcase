# Recurso: Privacidade e Consentimento (Self-Service LGPD)

## DescriÃ§Ã£o
Portal dedicado ao cidadÃ£o, permitindo que o titular dos dados exerÃ§a seus direitos previstos no Art. 18 da LGPD de forma autÃ´noma e transparente.

## Funcionalidades
1. ðŸ“œ **VisualizaÃ§Ã£o da PolÃ­tica**: PÃ¡gina interativa e navegÃ¡vel com a Ãºltima versÃ£o da PolÃ­tica de Privacidade.
2. âœ… **GestÃ£o de Consentimentos**: Lista de todos os termos aceitos pelo usuÃ¡rio, com data, IP e versÃ£o da polÃ­tica.
3. âŒ **RevogaÃ§Ã£o de Consentimento**: BotÃ£o para solicitar a interrupÃ§Ã£o do tratamento de dados para finalidades especÃ­ficas.
4. ðŸ“‚ **Portabilidade de Dados**: ExportaÃ§Ã£o de todos os dados do cidadÃ£o (cadastros, laudos, fotos) em um pacote `.zip` estruturado.
5. ðŸ•°ï¸ **TransparÃªncia Ativa (Meus Logs)**: HistÃ³rico de quando e como o sistema acessou os dados do prÃ³prio usuÃ¡rio.

## Fluxo de Portabilidade
1. O usuÃ¡rio clica em "Exportar Meus Dados".
2. O servidor gera um arquivo ZIP contendo:
   - Arquivo JSON com dados cadastrais.
   - Pasta `docs/` com cÃ³pias dos laudos enviados.
   - Foto do perfil.
3. O download Ã© iniciado automaticamente.
4. Uma entrada Ã© gerada no log de auditoria global registrando a exportaÃ§Ã£o.

## CritÃ©rios de AceitaÃ§Ã£o
- [ ] A revogaÃ§Ã£o de consentimentos essenciais deve exibir um alerta sobre a impossibilidade de continuar usando o serviÃ§o.
- [ ] O pacote de portabilidade deve vir criptografado ou em canal seguro HTTPS.
- [ ] A pÃ¡gina de polÃ­tica de privacidade deve ser acessÃ­vel mesmo sem login.

