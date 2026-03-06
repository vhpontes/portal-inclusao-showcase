# 🌻 Portal CIPTEA e Cordão de Girassol - PMPC

> **Secretaria Municipal de Saúde - Poços de Caldas/MG**  
> Sistema unificado para gestão de Carteiras de Identificação da Pessoa com Transtorno do Espectro Autista (CIPTEA) e Cordão de Girassol.

![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-yellow)
![Vue](https://img.shields.io/badge/Frontend-Vue.js%203-4FC08D)
![PHP](https://img.shields.io/badge/Backend-PHP%208-777BB4)
![Docs](https://img.shields.io/badge/Docs-Docsify-42b983)

---

## 📋 Sobre o Projeto
Este portal moderniza o cadastro e gestão de beneficiários, substituindo sistemas legados por uma arquitetura robusta e escalável. O sistema inclui ferramentas avançadas para migração de dados, validação em lote e mapeamento visual de campos de banco de dados.

## 🚀 Funcionalidades Principais

### 1. Centro de Migração (`/admin/migration`)
Ferramenta completa para transição de dados do sistema antigo:
- **Visual Mapper**: Interface "Drag-and-Drop" avançada com filtragem automática de tabelas legado (`legado_`).
- **Validação em Lote**: Higienização e validação de CPFs/Nomes massiva.
- **Configuração Flexível**: Gerenciamento de múltiplos ambientes (Local/Sede) com atalhos rápidos de conexão IP.
- **Comparação Inline**: Auditoria visual de dados legado vs sistema novo.

### 2. Acessibilidade e Privacidade
- **Linguagem Simples**: Página dedicada de Política de Privacidade otimizada para leitura fácil e acessível (`/privacidade-simples`).
- **Conformidade LGPD**: Transparência sobre coleta e uso de dados sensíveis.

### 2. Painel Administrativo
- **Dashboard**: Visão geral de cadastros e status.
- **Auditoria**: Logs detalhados de todas as operações de migração e edição.

### 3. Documentação Técnica
- Servidor de documentação integrado via **Docsify**.
- Diagramas de Arquitetura (Mermaid.js).
- Estatísticas de Código em tempo real.

---

## 🛠️ Stack Tecnológica

| Camada | Tecnologias |
| :--- | :--- |
| **Frontend** | Vue 3, Vite, TailwindCSS v4, Vue Flow, Axios |
| **Backend** | PHP 8.x (PSR-4), Router Regex, JWT Nativo |
| **Padrões** | Repository Pattern, Middlewares, Dependency Proxy |
| **Banco de Dados** | MySQL / MariaDB (Schema Normalizado) |
| **Docs** | OpenAPI/Swagger, Docsify |

---

## ⚙️ Instalação e Execução

### Pré-requisitos
- **XAMPP/WAMP** (Apache + MySQL + PHP 8+)
- **Node.js** (v18+)

### 1. Configuração do Banco
Execute os scripts SQL na ordem:
1. `database/schema.sql` (Estrutura)
2. `database/seed_admin.sql` (Usuário Admin Inicial)

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

### 4. Servidor de Documentação
Para rodar a documentação localmente:
```powershell
# Executar via PowerShell na pasta docs/
./server.ps1
```
Acesse em: `http://localhost:3001`

---

## 📂 Estrutura do Projeto

```
├── api/                  # Backend PHP Modernizado
│   ├── src/              # Código Fonte (PSR-4: App\)
│   ├── config/           # Configurações de ambiente
│   ├── scripts/          # Utilitários e Migração
│   ├── storage/          # Logs e Cache
│   └── tests/            # Testes Unitários e Diagnóstico
├── frontend/             # SPA Vue.js 3
├── docs/                 # Documentação (Swagger, Docsify)
├── scripts/              # Automação Shell (.bat, .ps1)
└── database/             # Schemas SQL e Seeders

---
### 🕰️ Histórico de Atualizações
| Data | Versão | Resumo | Autor |
| :--- | :--- | :--- | :--- |
| 18/02/2026 11:30 | 1.2 | Faxina técnica na raiz, organização de arquivos e migração para Composer Autoloader. | Victor Hugo Manata Pontes |
| 18/02/2026 09:00 | 1.1 | Otimização de performance: Indexação SQL, CacheService e LogService. | Victor Hugo Manata Pontes |
| 14/02/2026 16:00 | 0.8 | Refatoração de Alertas para Modais e padronização visual. | Victor Hugo Manata Pontes |
| 09/02/2026 10:00 | 0.6 | Dashboard BI: Heatmap de beneficiários e funcionalidades de expansão. | Victor Hugo Manata Pontes |
| 08/02/2026 09:00 | 0.5 | Refatoração do Cadastro (Wizard) e melhorias de Acessibilidade (WCAG 2.2). | Victor Hugo Manata Pontes |
| 01/02/2026 14:00 | 0.1 | Suporte inicial a PDF Renderer com rotação de texto. | Victor Hugo Manata Pontes |

---

## 👤 Autor
**Victor Hugo Manata Pontes**  
Secretaria Municipal de Saúde - PMPC  
*Desenvolvido em conformidade com a LGPD e Direitos da Pessoa com Deficiência.*