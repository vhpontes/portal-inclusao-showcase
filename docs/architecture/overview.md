# VisÃ£o Geral da Arquitetura - Portal de InclusÃ£o

## IntroduÃ§Ã£o

O **Portal de InclusÃ£o** Ã© um sistema web para gestÃ£o de solicitaÃ§Ãµes de carteiras de identificaÃ§Ã£o (CIPTEA e Girassol) para pessoas com deficiÃªncia no municÃ­pio de PoÃ§os de Caldas/MG. O sistema inclui funcionalidades avanÃ§adas de migraÃ§Ã£o de dados de sistemas legados.

## Diagrama C4 - NÃ­vel 1: Contexto do Sistema

```mermaid
C4Context
    title Diagrama de Contexto - Portal de InclusÃ£o
    
    Person(cidadao, "CidadÃ£o", "Pessoa com deficiÃªncia ou responsÃ¡vel legal")
    Person(admin, "Administrador", "Servidor municipal da Secretaria de SaÃºde")
    
    System(portal, "Portal de InclusÃ£o", "Sistema web para solicitaÃ§Ã£o e gestÃ£o de carteiras CIPTEA/Girassol")
    
    System_Ext(legado, "Sistema Legado", "Banco de dados do sistema anterior (Cadastro)")
    System_Ext(n8n, "n8n Workflow", "AutomaÃ§Ã£o de processos e integraÃ§Ãµes")
    System_Ext(email, "Servidor de Email", "Envio de notificaÃ§Ãµes")
    
    Rel(cidadao, portal, "Solicita carteira, acompanha status", "HTTPS")
    Rel(admin, portal, "Gerencia solicitaÃ§Ãµes, aprova/rejeita", "HTTPS")
    Rel(portal, legado, "Migra dados histÃ³ricos", "MySQL/TCP")
    Rel(portal, n8n, "Dispara workflows", "HTTP/Webhook")
    Rel(portal, email, "Envia notificaÃ§Ãµes", "SMTP")
```

## Diagrama C4 - NÃ­vel 2: ContÃªineres

```mermaid
C4Container
    title Diagrama de ContÃªineres - Portal de InclusÃ£o
    
    Person(user, "UsuÃ¡rio", "CidadÃ£o ou Administrador")
    
    Container_Boundary(portal, "Portal de InclusÃ£o") {
        Container(spa, "Frontend SPA", "Vue.js 3, Vite", "Interface web responsiva com componentes reativos")
        Container(api, "Backend API", "PHP 8+, PDO", "API REST para lÃ³gica de negÃ³cio e acesso a dados")
        ContainerDb(db, "Banco de Dados", "MySQL 8.0", "Armazena usuÃ¡rios, beneficiÃ¡rios, solicitaÃ§Ãµes e documentos")
    }
    
    System_Ext(legado_db, "Banco Legado", "MySQL - Sistema anterior")
    System_Ext(storage, "File Storage", "Sistema de arquivos local")
    
    Rel(user, spa, "Acessa via navegador", "HTTPS")
    Rel(spa, api, "Faz requisiÃ§Ãµes", "JSON/REST")
    Rel(api, db, "LÃª/Escreve dados", "PDO/MySQL")
    Rel(api, legado_db, "Migra dados histÃ³ricos", "PDO/MySQL")
    Rel(api, storage, "Armazena uploads", "File System")
```

## Diagrama de Componentes - Backend API

```mermaid
graph TB
    subgraph "Backend API (PHP)"
        Router[index.php<br/>Router] --> Controllers{Controllers}
        
        Controllers --> AuthCtrl[AuthController<br/>Login/AutenticaÃ§Ã£o]
        Controllers --> ReqCtrl[RequestController<br/>SolicitaÃ§Ãµes CRUD]
        Controllers --> MigCtrl[MigrationController<br/>MigraÃ§Ã£o de Dados]
        Controllers --> DashCtrl[DashboardController<br/>EstatÃ­sticas]
        Controllers --> RepCtrl[ReportController<br/>RelatÃ³rios]
        Controllers --> SecCtrl[SecurityController<br/>Criptografia]
        
        MigCtrl --> MigSvc[MigrationService<br/>LÃ³gica de MigraÃ§Ã£o]
        
        AuthCtrl --> DB[(Database)]
        ReqCtrl --> DB
        MigCtrl --> DB
        DashCtrl --> DB
        RepCtrl --> DB
        
        MigSvc --> LegacyDB[(Banco Legado)]
        MigSvc --> DB
    end
    
    SPA[Frontend Vue.js] -->|JSON/REST| Router
    
    style MigCtrl fill:#e1f5ff
    style MigSvc fill:#e1f5ff
    style LegacyDB fill:#ffe1e1
```

## Diagrama de Componentes - Frontend SPA

```mermaid
graph TB
    subgraph "Frontend Vue.js"
        Router[Vue Router] --> Views{Views}
        
        Views --> Home[Home.vue<br/>PÃ¡gina Inicial]
        Views --> Login[AdminLogin.vue<br/>Login Admin]
        Views --> Dashboard[AdminDashboard.vue<br/>Painel Admin]
        Views --> Migration[AdminMigration.vue<br/>Centro de MigraÃ§Ã£o]
        Views --> Mapper[AdminMapper.vue<br/>Mapeador Visual]
        Views --> ReqDetails[AdminRequestDetails.vue<br/>Detalhes SolicitaÃ§Ã£o]
        
        Migration --> DebugConsole[DebugConsole.vue<br/>Console de Debug]
        Migration --> BaseModal[BaseModal.vue<br/>Modal ReutilizÃ¡vel]
        Migration --> Encryption[AdminEncryption.vue<br/>Teste Criptografia]
        
        Mapper --> TableNode[TableNode.vue<br/>NÃ³ de Tabela]
        Mapper --> VueFlow[VueFlow Library<br/>Drag & Drop]
    end
    
    API[Backend API] -->|JSON| Router
    
    style Migration fill:#fff4e1
    style Mapper fill:#e1ffe1
```

## Tecnologias Utilizadas

### Frontend
| Tecnologia | VersÃ£o | PropÃ³sito |
|------------|--------|-----------|
| Vue.js | 3.x | Framework progressivo para UI reativa |
| Vite | 5.x | Build tool e dev server rÃ¡pido |
| Vue Router | 4.x | Roteamento SPA |
| TailwindCSS | 3.x | Framework CSS utilitÃ¡rio |
| @vue-flow/core | Latest | Biblioteca para diagramas interativos |

### Backend
| Tecnologia | VersÃ£o | PropÃ³sito |
|------------|--------|-----------|
| PHP | 8.0+ | Linguagem server-side |
| PDO | Nativo | AbstraÃ§Ã£o de banco de dados |
| OpenSSL | Nativo | Criptografia AES-256-CBC |

### Banco de Dados
| Tecnologia | VersÃ£o | PropÃ³sito |
|------------|--------|-----------|
| MySQL | 8.0 | Banco de dados relacional |
| Host | pocos-acolhedora-srv | Servidor de produÃ§Ã£o |

### Infraestrutura
| Componente | Tecnologia | PropÃ³sito |
|------------|------------|-----------|
| Web Server | PHP Built-in | Desenvolvimento (porta 8000) |
| Frontend Server | Vite Dev | Desenvolvimento (porta 5173) |
| Debug Console | DebugConsole.vue | Monitoramento de conexÃ£o DB |

## Fluxo de Dados Principal

```mermaid
sequenceDiagram
    participant U as UsuÃ¡rio
    participant F as Frontend (Vue)
    participant A as API (PHP)
    participant D as Database (MySQL)
    
    U->>F: Acessa aplicaÃ§Ã£o
    F->>A: GET /api/status
    A->>D: Verifica conexÃ£o
    D-->>A: Status OK
    A-->>F: {status: "ok"}
    F-->>U: Exibe interface
    
    U->>F: Submete solicitaÃ§Ã£o
    F->>A: POST /api/solicitacoes
    A->>D: INSERT INTO solicitacoes
    A->>D: INSERT INTO documentos
    D-->>A: IDs criados
    A-->>F: {success: true, protocolo: "..."}
    F-->>U: ConfirmaÃ§Ã£o + Protocolo
```

## PadrÃµes Arquiteturais

### Backend
- **PSR-4 Autoloading**: Carregamento automÃ¡tico de classes via namespaces (`App\`).
- **Repository Pattern**: Isolamento total do SQL, facilitando a testabilidade e troca de banco.
- **Service Layer**: CentralizaÃ§Ã£o da lÃ³gica de negÃ³cio (ex: `RequestService`, `DashboardService`).
- **Middleware System**: InterceptaÃ§Ã£o de rotas para seguranÃ§a e validaÃ§Ã£o (JWT).
- **JWT Authentication**: Sistema de tokens HS256 real para proteÃ§Ã£o de Ã¡reas administrativas.
- **MVC Simplificado**: Estrutura organizada para escalabilidade.

### Frontend
- **Component-Based**: Componentes Vue reutilizÃ¡veis
- **Single Page Application**: NavegaÃ§Ã£o sem reload
- **Reactive State**: Refs e computed properties
- **Composition API**: Setup script para lÃ³gica de componentes

## SeguranÃ§a

### AutenticaÃ§Ã£o
- Login via email/senha
- Senha hash com `password_hash()` (bcrypt)
- Token armazenado em `localStorage`
- Guard de navegaÃ§Ã£o para rotas protegidas

### AutorizaÃ§Ã£o
- Perfis: `cidadao` e `admin`
- Rotas administrativas protegidas por `meta.requiresAuth`
- VerificaÃ§Ã£o de perfil no backend

### Criptografia
- **Algoritmo**: AES-256-CBC
- **Uso**: Dados sensÃ­veis do sistema legado (CPF, RG, CNS)
- **Chaves**: Definidas em `SecurityController` e `MigrationService`

## PrÃ³ximos Passos

Consulte a documentaÃ§Ã£o especÃ­fica:
- [Arquitetura de MigraÃ§Ã£o](migration_architecture.md)
- [Status de ModernizaÃ§Ã£o](modernization_status.md)
- [Schema do Banco de Dados](database_schema.md)
- [EspecificaÃ§Ã£o da API](../api/spec.yaml)

---
### ðŸ•°ï¸ HistÃ³rico de AtualizaÃ§Ãµes
| Data | VersÃ£o | Resumo | Autor |
| :--- | :--- | :--- | :--- |
| 18/02/2026 11:15 | 1.2 | RevisÃ£o da arquitetura apÃ³s migraÃ§Ã£o para Composer e limpeza tÃ©cnica. | Victor Hugo Manata Pontes |
| 18/02/2026 09:00 | 1.1 | Melhorias de performance: Caching e IndexaÃ§Ã£o SQL. | Victor Hugo Manata Pontes |
| 14/02/2026 16:00 | 0.8 | PadronizaÃ§Ã£o de Modais e Componentes UI. | Victor Hugo Manata Pontes |
| 09/02/2026 10:00 | 0.6 | Dashboard BI e MÃ³dulo de Geoprocessamento. | Victor Hugo Manata Pontes |
| 08/02/2026 09:00 | 0.5 | Landing Page e Wizard de Cadastro. | Victor Hugo Manata Pontes |
| 01/02/2026 14:00 | 0.1 | DefiniÃ§Ã£o inicial da arquitetura de containers. | Victor Hugo Manata Pontes |

