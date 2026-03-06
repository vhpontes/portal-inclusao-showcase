# Inteligência Artificial e Biometria

O Portal de Inclusão utiliza tecnologias avançadas de visão computacional e processamento de documentos para garantir a autenticidade dos dados e facilitar o processo de inscrição.

## 📸 Biometria Facial OACI

Implementamos um motor de validação facial baseado no **MediaPipe**, que avalia a foto 3x4 do cidadão em tempo real.

### Critérios de Validação
- **Presença de Rosto**: Verifica se há uma face humana detectável.
- **Centralização**: Garante que o rosto está posicionado corretamente no quadro.
- **Qualidade de Imagem**: Analisa a nitidez (blur) e iluminação.
- **Acessórios**: Detecta o uso de óculos escuros ou chapéus que possam dificultar a identificação.
- **Enquadramento 3x4**: Ferramenta de recorte profissional integrada (Cropper.js).
- **Visualização Ampliada**: Clique sobre a foto ou documentos carregados para abri-los em tela cheia para conferência.

## 📄 Motor de OCR (Tesseract.js)

O sistema é capaz de "ler" documentos em formato de imagem e PDF para extrair informações críticas.

### Funcionalidades
- **Suporte a PDF**: Conversão automática da primeira página de PDFs para análise.
- **Feedback Visual**: Barra de progresso circular dinâmico que informa o status da leitura ao usuário.
- **Detecção de Tipo**: Identificação inteligente entre RG, CPF, CNH e Certidão de Nascimento.

## ⚖️ Regras de Negócio e CRM

### Validação de Laudo Médico
- **CID-10**: Localização automática de códigos internacionais de doenças.
- **CRM Médico**: Extração do registro profissional e consulta à **API Oficial do CFM** (Resolução nº 2.309/2022).
- **Titularidade**: Comparação entre o nome no documento e o nome preenchido no formulário.

### Validação de Identidade
- **Conferência de Dados**: O sistema verifica se o CPF e RG lidos no documento coincidem exatamente com o informado no Passo 2.
- **Atalhos Navigacionais**: Mensagens de erro contêm links rápidos para voltar à etapa de Dados Pessoais caso necessário.

## 🏗️ Navegação e Segurança

- **Stepper Interativo**: Permite a navegação fluida entre os passos (0 a 4) com travas de segurança que impedem o avanço sem validação de IA.
- **Sessão Administrativa**: Gestão dinâmica de cabeçalho com nome do usuário operante e botão de logoff.
