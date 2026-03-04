# ðŸŒ» Portal CIPTEA e CordÃ£o de Girassol - PMPC

> **Secretaria Municipal de SaÃºde - PoÃ§os de Caldas/MG**  
> Sistema unificado para gestÃ£o de Carteiras de IdentificaÃ§Ã£o da Pessoa com Transtorno do Espectro Autista (CIPTEA) e CordÃ£o de Girassol.

![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-yellow)
![Vue](https://img.shields.io/badge/Frontend-Vue.js%203-4FC08D)
![PHP](https://img.shields.io/badge/Backend-PHP%208-777BB4)
![Docs](https://img.shields.io/badge/Docs-Docsify-42b983)

---

## ðŸ“‹ Sobre o Projeto
Este portal moderniza o cadastro e gestÃ£o de beneficiÃ¡rios, substituindo sistemas legados por uma arquitetura robusta e escalÃ¡vel. O sistema inclui ferramentas avanÃ§adas para migraÃ§Ã£o de dados, validaÃ§Ã£o em lote e mapeamento visual de campos de banco de dados.

## ðŸš€ Funcionalidades Principais

### 1. Centro de MigraÃ§Ã£o (`/admin/migration`)
Ferramenta completa para transiÃ§Ã£o de dados do sistema antigo:
- **Visual Mapper**: Interface "Drag-and-Drop" avanÃ§ada com filtragem automÃ¡tica de tabelas legado (`legado_`).
- **ValidaÃ§Ã£o em Lote**: HigienizaÃ§Ã£o e validaÃ§Ã£o de CPFs/Nomes massiva.
- **ConfiguraÃ§Ã£o FlexÃ­vel**: Gerenciamento de mÃºltiplos ambientes (Local/Sede) com atalhos rÃ¡pidos de conexÃ£o IP.
- **ComparaÃ§Ã£o Inline**: Auditoria visual de dados legado vs sistema novo.

### 2. Acessibilidade e Privacidade
- **Linguagem Simples**: PÃ¡gina dedicada de PolÃ­tica de Privacidade otimizada para leitura fÃ¡cil e acessÃ­vel (`/privacidade-simples`).
- **Conformidade LGPD**: TransparÃªncia sobre coleta e uso de dados sensÃ­veis.

### 2. Painel Administrativo
- **Dashboard**: VisÃ£o geral de cadastros e status.
- **Auditoria**: Logs detalhados de todas as operaÃ§Ãµes de migraÃ§Ã£o e ediÃ§Ã£o.

### 3. DocumentaÃ§Ã£o TÃ©cnica
- Servidor de documentaÃ§Ã£o integrado via **Docsify**.
- Diagramas de Arquitetura (Mermaid.js).
- EstatÃ­sticas de CÃ³digo em tempo real.

---

## ðŸ› ï¸ Stack TecnolÃ³gica

| Camada | Tecnologias |
| :--- | :--- |
| **Frontend** | Vue 3, Vite, TailwindCSS v4, Vue Flow, Axios |
| **Backend** | PHP 8.x (PSR-4), Router Regex, JWT Nativo |
| **PadrÃµes** | Repository Pattern, Middlewares, Dependency Proxy |
| **Banco de Dados** | MySQL / MariaDB (Schema Normalizado) |
| **Docs** | OpenAPI/Swagger, Docsify |

---

1. `database/schema.sql` (Estrutura)
2. `database/seed_admin.sql` (UsuÃ¡rio Admin Inicial)

### 2. Backend (API)
Configure o arquivo `api/config/database.php` com suas credenciais:
```php
define('DB_HOST', 'localhost');
define('DB_NAME', 'pmpc_ciptea');
define('DB_USER', 'root');
define('DB_PASS', '');
```

### 3. Frontend
```bash
cd frontend
npm install
npm run dev
```

### 4. Servidor de DocumentaÃ§Ã£o
Para rodar a documentaÃ§Ã£o localmente:
```powershell
# Executar via PowerShell na pasta docs/
./server.ps1
```
Acesse em: `http://localhost:3001`

---

## ðŸ“‚ Estrutura do Projeto

```
â”œâ”€â”€ api/                  # Backend PHP Modernizado
â”‚   â”œâ”€â”€ src/              # CÃ³digo Fonte (PSR-4: App\)
â”‚   â”œâ”€â”€ config/           # ConfiguraÃ§Ãµes de ambiente
â”‚   â”œâ”€â”€ scripts/          # UtilitÃ¡rios e MigraÃ§Ã£o
â”‚   â”œâ”€â”€ storage/          # Logs e Cache
â”‚   â””â”€â”€ tests/            # Testes UnitÃ¡rios e DiagnÃ³stico
â”œâ”€â”€ frontend/             # SPA Vue.js 3
â”œâ”€â”€ docs/                 # DocumentaÃ§Ã£o (Swagger, Docsify)
â”œâ”€â”€ scripts/              # AutomaÃ§Ã£o Shell (.bat, .ps1)
â””â”€â”€ database/             # Schemas SQL e Seeders

---
### ðŸ•°ï¸ HistÃ³rico de AtualizaÃ§Ãµes
| Data | VersÃ£o | Resumo | Autor |
| :--- | :--- | :--- | :--- |
| 18/02/2026 11:30 | 1.2 | Faxina tÃ©cnica na raiz, organizaÃ§Ã£o de arquivos e migraÃ§Ã£o para Composer Autoloader. | Victor Hugo Manata Pontes |
| 18/02/2026 09:00 | 1.1 | OtimizaÃ§Ã£o de performance: IndexaÃ§Ã£o SQL, CacheService e LogService. | Victor Hugo Manata Pontes |
| 14/02/2026 16:00 | 0.8 | RefatoraÃ§Ã£o de Alertas para Modais e padronizaÃ§Ã£o visual. | Victor Hugo Manata Pontes |
| 09/02/2026 10:00 | 0.6 | Dashboard BI: Heatmap de beneficiÃ¡rios e funcionalidades de expansÃ£o. | Victor Hugo Manata Pontes |
| 08/02/2026 09:00 | 0.5 | RefatoraÃ§Ã£o do Cadastro (Wizard) e melhorias de Acessibilidade (WCAG 2.2). | Victor Hugo Manata Pontes |
| 01/02/2026 14:00 | 0.1 | Suporte inicial a PDF Renderer com rotaÃ§Ã£o de texto. | Victor Hugo Manata Pontes |

---

## ðŸ‘¤ Autor
**Victor Hugo Manata Pontes**  
Secretaria Municipal de SaÃºde - PMPC  
*Desenvolvido em conformidade com a LGPD e Direitos da Pessoa com DeficiÃªncia.*
