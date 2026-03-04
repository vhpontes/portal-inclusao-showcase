# InteligÃªncia Artificial e Biometria

O Portal de InclusÃ£o utiliza tecnologias avanÃ§adas de visÃ£o computacional e processamento de documentos para garantir a autenticidade dos dados e facilitar o processo de inscriÃ§Ã£o.

## ðŸ“¸ Biometria Facial OACI

Implementamos um motor de validaÃ§Ã£o facial baseado no **MediaPipe**, que avalia a foto 3x4 do cidadÃ£o em tempo real.

### CritÃ©rios de ValidaÃ§Ã£o
- **PresenÃ§a de Rosto**: Verifica se hÃ¡ uma face humana detectÃ¡vel.
- **CentralizaÃ§Ã£o**: Garante que o rosto estÃ¡ posicionado corretamente no quadro.
- **Qualidade de Imagem**: Analisa a nitidez (blur) e iluminaÃ§Ã£o.
- **AcessÃ³rios**: Detecta o uso de Ã³culos escuros ou chapÃ©us que possam dificultar a identificaÃ§Ã£o.
- **Enquadramento 3x4**: Ferramenta de recorte profissional integrada (Cropper.js).
- **VisualizaÃ§Ã£o Ampliada**: Clique sobre a foto ou documentos carregados para abri-los em tela cheia para conferÃªncia.

## ðŸ“„ Motor de OCR (Tesseract.js)

O sistema Ã© capaz de "ler" documentos em formato de imagem e PDF para extrair informaÃ§Ãµes crÃ­ticas.

### Funcionalidades
- **Suporte a PDF**: ConversÃ£o automÃ¡tica da primeira pÃ¡gina de PDFs para anÃ¡lise.
- **Feedback Visual**: Barra de progresso circular dinÃ¢mico que informa o status da leitura ao usuÃ¡rio.
- **DetecÃ§Ã£o de Tipo**: IdentificaÃ§Ã£o inteligente entre RG, CPF, CNH e CertidÃ£o de Nascimento.

## âš–ï¸ Regras de NegÃ³cio e CRM

### ValidaÃ§Ã£o de Laudo MÃ©dico
- **CID-10**: LocalizaÃ§Ã£o automÃ¡tica de cÃ³digos internacionais de doenÃ§as.
- **CRM MÃ©dico**: ExtraÃ§Ã£o do registro profissional e consulta Ã  **API Oficial do CFM** (ResoluÃ§Ã£o nÂº 2.309/2022).
- **Titularidade**: ComparaÃ§Ã£o entre o nome no documento e o nome preenchido no formulÃ¡rio.

### ValidaÃ§Ã£o de Identidade
- **ConferÃªncia de Dados**: O sistema verifica se o CPF e RG lidos no documento coincidem exatamente com o informado no Passo 2.
- **Atalhos Navigacionais**: Mensagens de erro contÃªm links rÃ¡pidos para voltar Ã  etapa de Dados Pessoais caso necessÃ¡rio.

## ðŸ—ï¸ NavegaÃ§Ã£o e SeguranÃ§a

- **Stepper Interativo**: Permite a navegaÃ§Ã£o fluida entre os passos (0 a 4) com travas de seguranÃ§a que impedem o avanÃ§o sem validaÃ§Ã£o de IA.
- **SessÃ£o Administrativa**: GestÃ£o dinÃ¢mica de cabeÃ§alho com nome do usuÃ¡rio operante e botÃ£o de logoff.

