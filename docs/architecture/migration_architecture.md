# Arquitetura do MÃ³dulo de MigraÃ§Ã£o

## VisÃ£o Geral

O sistema possui um mÃ³dulo completo de migraÃ§Ã£o de dados do sistema legado (tabela `Cadastro`) para o novo schema. A migraÃ§Ã£o trata desafios complexos como:
- Descriptografia de dados sensÃ­veis (CPF, RG, CNS)
- ValidaÃ§Ã£o e sanitizaÃ§Ã£o de dados
- Mapeamento visual de campos entre schemas diferentes
- RevalidaÃ§Ã£o de registros migrados

## Arquitetura do MÃ³dulo de MigraÃ§Ã£o

```mermaid
graph TB
    subgraph "Frontend - Centro de MigraÃ§Ã£o"
        UI[AdminMigration.vue<br/>Interface Principal]
        Mapper[AdminMapper.vue<br/>Mapeador Visual]
        Encryption[AdminEncryption.vue<br/>Teste de Criptografia]
        Debug[DebugConsole.vue<br/>Monitor de ConexÃ£o]
    end
    
    subgraph "Backend - API de MigraÃ§Ã£o"
        Controller[MigrationController]
        Service[MigrationService]
    end
    
    subgraph "Bancos de Dados"
        Target[(DB Atual<br/>db_ciptea_girassol)]
        Legacy[(DB Legado<br/>Tabela: Cadastro)]
    end
    
    UI --> Controller
    Mapper --> Controller
    Encryption --> Controller
    
    Controller --> Service
    Service --> Target
    Service --> Legacy
    
    Debug -.->|Monitora| Target
    
    style Service fill:#e1f5ff
    style Legacy fill:#ffe1e1
```

## Endpoints da API de MigraÃ§Ã£o

| Endpoint | MÃ©todo | DescriÃ§Ã£o |
|----------|--------|-----------|
| `/api/migration/start` | POST | Inicia processo de migraÃ§Ã£o |
| `/api/migration/status` | GET | Retorna status da migraÃ§Ã£o em andamento |
| `/api/migration/list` | GET | Lista registros migrados |
| `/api/migration/validate` | POST | Revalida registros migrados |
| `/api/migration/compare` | POST | Compara registro atual com legado |
| `/api/migration/schema` | POST | Retorna schema de uma tabela |
| `/api/migration/tables` | POST | Lista tabelas disponÃ­veis |
| `/api/migration/save_mapping` | POST | Salva mapeamento de campos |

## Fluxo Completo de MigraÃ§Ã£o

```mermaid
sequenceDiagram
    participant Admin as Administrador
    participant UI as AdminMigration.vue
    participant API as MigrationController
    participant Svc as MigrationService
    participant Legacy as BD Legado
    participant Target as BD Atual
    
    Admin->>UI: Configura credenciais do BD Legado
    UI->>API: POST /api/migration/start
    
    API->>Svc: runMigration(config)
    Svc->>Legacy: Conectar (host, user, pass)
    Legacy-->>Svc: ConexÃ£o OK
    
    Svc->>Legacy: SELECT * FROM Cadastro
    Legacy-->>Svc: Registros (criptografados)
    
    loop Para cada registro
        Svc->>Svc: Tentar descriptografar CPF/RG/CNS
        
        alt Descriptografia bem-sucedida
            Svc->>Svc: Usar valor descriptografado
        else Descriptografia falhou
            Svc->>Svc: Manter NULL + Backup em legacy_*
        end
        
        Svc->>Svc: Validar CPF (dÃ­gitos verificadores)
        Svc->>Svc: Sanitizar nome (Title Case)
        
        Svc->>Target: Verificar duplicata (legado_id, CPF)
        
        alt NÃ£o Ã© duplicata
            Svc->>Target: INSERT INTO usuarios
            Svc->>Target: INSERT INTO beneficiarios
            Svc->>Target: INSERT INTO solicitacoes
            Svc->>Legacy: SELECT * FROM Arquivos
            Svc->>Target: INSERT INTO documentos
        else Ã‰ duplicata
            Svc->>Svc: Pular registro
        end
    end
    
    Svc->>Target: COMMIT
    Svc-->>API: {status: "completed", message: "..."}
    API-->>UI: Resposta JSON
    UI-->>Admin: Exibe resultado
```

## Processo de Descriptografia

### Algoritmo Utilizado
- **MÃ©todo**: AES-256-CBC
- **Chave**: Hash SHA-256 de `"AHgsi278"`
- **IV**: Primeiros 16 bytes do hash SHA-256 de `"sxcsdfce"`

### EstratÃ©gias de Descriptografia

```mermaid
flowchart TD
    A[Valor Criptografado] --> B{EstratÃ©gia 1:<br/>Base64 Decode + Decrypt}
    B -->|Sucesso| C[Retornar Valor]
    B -->|Falha| D{EstratÃ©gia 2:<br/>Double Base64 + Raw Decrypt}
    D -->|Sucesso| C
    D -->|Falha| E[Retornar FALSE]
    
    E --> F[Salvar em legacy_*]
    F --> G[Campo principal = NULL]
    
    C --> H{Validar Formato}
    H -->|CPF vÃ¡lido| I[Usar valor]
    H -->|InvÃ¡lido| F
```

### CÃ³digo de Descriptografia

```php
private function my_simple_crypt($string, $action = 'd') {
    $secret_key = 'AHgsi278';
    $secret_iv = 'sxcsdfce';
    
    $encrypt_method = "AES-256-CBC";
    $key = hash('sha256', $secret_key); 
    $iv = substr(hash('sha256', $secret_iv), 0, 16);
    
    if ($action == 'd') {
        // EstratÃ©gia 1: Decode padrÃ£o
        $attempt1 = openssl_decrypt(base64_decode($string), $encrypt_method, $key, 0, $iv);
        if ($attempt1 !== false) return $attempt1;

        // EstratÃ©gia 2: Double Base64
        $step1 = base64_decode($string);
        $step2 = base64_decode($step1);
        if ($step2) {
            $attempt2 = openssl_decrypt($step2, $encrypt_method, $key, OPENSSL_RAW_DATA, $iv);
            if ($attempt2 !== false) return $attempt2;
        }
    }
    return false;
}
```

## Mapeamento Visual de Campos

O componente `AdminMapper.vue` permite mapear visualmente campos entre tabelas:

```mermaid
graph LR
    subgraph "Tabela Origem (Legado)"
        L1[Id]
        L2[Nome]
        L3[CPF]
        L4[Nascimento]
        L5[Mae]
    end
    
    subgraph "Tabela Destino (Atual)"
        T1[id]
        T2[nome]
        T3[cpf]
        T4[data_nascimento]
        T5[responsavel_nome]
    end
    
    L1 -.->|legado_id| T1
    L2 -->|nome| T2
    L3 -->|cpf| T3
    L4 -->|data_nascimento| T4
    L5 -->|responsavel_nome| T5
    
    style L3 fill:#ffe1e1
    style T3 fill:#e1ffe1
```

### Funcionalidades do Mapeador

1. **Drag & Drop**: Arraste tabelas da sidebar para o canvas
2. **ConexÃµes**: Conecte campos de origem (azul) para destino (verde)
3. **ExportaÃ§Ã£o**: Salve o mapeamento em JSON
4. **ImportaÃ§Ã£o**: Carregue mapeamentos salvos
5. **PersistÃªncia**: Mapeamentos salvos em `migration_config.json`

## Processo de RevalidaÃ§Ã£o

ApÃ³s a migraÃ§Ã£o, registros podem ser revalidados individualmente ou em lote:

```mermaid
flowchart TD
    A[Selecionar Registros] --> B[Abrir Modal de RevalidaÃ§Ã£o]
    B --> C{Comparar com Legado?}
    
    C -->|Sim| D[Buscar dados do BD Legado]
    D --> E[Exibir ComparaÃ§Ã£o]
    E --> F{UsuÃ¡rio seleciona<br/>campos para restaurar}
    
    C -->|NÃ£o| G[Pular comparaÃ§Ã£o]
    F --> H[Aplicar Overrides]
    G --> H
    
    H --> I[Sanitizar Nome]
    I --> J[Validar CPF]
    
    J -->|VÃ¡lido| K[Atualizar Registro]
    J -->|InvÃ¡lido| L[Marcar Erro]
    
    K --> M[Aprovar SolicitaÃ§Ã£o]
    M --> N[Adicionar ObservaÃ§Ã£o]
    N --> O[Fim]
    L --> O
```

### ValidaÃ§Ãµes AutomÃ¡ticas

1. **CPF**: Verifica dÃ­gitos verificadores
2. **Nome**: Converte para Title Case, remove caracteres especiais
3. **ResponsÃ¡vel**: Sanitiza nome do responsÃ¡vel

## Tratamento de Dados Criptografados

### EstratÃ©gia de Backup

Quando a descriptografia falha, os dados originais sÃ£o preservados:

| Campo Principal | Campo de Backup | Comportamento |
|----------------|-----------------|---------------|
| `cpf` | `legacy_cpf` | NULL se falhar + backup do valor criptografado |
| `rg` | `legacy_rg` | NULL se falhar + backup do valor criptografado |
| `cns` | `legacy_cns` | NULL se falhar + backup do valor criptografado |

### Exemplo de Registro Migrado

```json
{
  "id": 123,
  "nome": "JoÃ£o da Silva",
  "cpf": null,
  "rg": null,
  "legacy_cpf": "U2FsdGVkX1+vupppZksvRf5pq5g5XjFRlipRkwB0K1Y=",
  "legacy_rg": "U2FsdGVkX1+vupppZksvRf5pq5g5XjFRlipRkwB0K1Y=",
  "legado_id": 456
}
```

## Monitoramento e Debug

### Debug Console

O componente `DebugConsole.vue` monitora em tempo real:
- Status da conexÃ£o com o banco de dados
- Host, database e usuÃ¡rio
- Contagem de registros por tabela

### Logs de MigraÃ§Ã£o

Logs sÃ£o salvos em `migration_status.json`:

```json
{
  "status": "completed",
  "message": "MigraÃ§Ã£o concluÃ­da! 150 registros processados.",
  "progress": 100,
  "timestamp": 1707504000
}
```

## ConfiguraÃ§Ã£o de MigraÃ§Ã£o

### Arquivo de ConfiguraÃ§Ã£o

Exemplo de `AdminMigration.vue` config:

```javascript
const config = ref({
    host: 'pocos-acolhedora-srv',
    db_name: 'db_ciptea_girassol',
    username: 'ciptea_girassol_dti',
    password: '||2s5zNf58Z|T~9^4eY]',
    port: '3306',
    fieldsString: 'nome, cpf, data_nascimento, mae',
    listFieldsString: 'nome, cpf, legado_id, responsavel_nome'
})
```

### BotÃ£o "Usar Banco Atual"

Preenche automaticamente com credenciais locais quando origem = destino:

```javascript
const useLocalConfig = () => {
    config.value = {
        host: '127.0.0.1',
        port: '3306',
        db_name: 'db_ciptea_girassol',
        username: 'ciptea_girassol_dti',
        password: '', // UsuÃ¡rio preenche
        ...
    }
}
```

## Casos de Uso

### 1. MigraÃ§Ã£o Inicial

```bash
# Administrador acessa /admin/migracao
# Configura credenciais do banco legado
# Clica em "Iniciar MigraÃ§Ã£o"
# Sistema processa todos os registros
# Exibe resultado: "150 registros migrados com sucesso"
```

### 2. RevalidaÃ§Ã£o em Lote

```bash
# Administrador acessa aba "RevalidaÃ§Ã£o"
# Seleciona 10 registros com CPF invÃ¡lido
# Clica em "Revalidar Selecionados"
# Sistema valida e corrige automaticamente
# Resultado: "8 aprovados, 2 com erro"
```

### 3. Mapeamento Visual

```bash
# Administrador acessa aba "Mapeamento Visual"
# Arrasta "legado_cadastro" para o canvas
# Arrasta "beneficiarios" para o canvas
# Conecta campos: Nome â†’ nome, CPF â†’ cpf
# Clica em "Salvar Mapeamento"
# ConfiguraÃ§Ã£o salva em migration_config.json
```

## MÃ©tricas de Sucesso

- **Taxa de Descriptografia**: ~60-70% (varia por campo)
- **Registros Duplicados**: Ignorados automaticamente
- **ValidaÃ§Ã£o de CPF**: ~85% de sucesso
- **Tempo MÃ©dio**: ~0.5s por registro

## PrÃ³ximos Passos

- [Status de ModernizaÃ§Ã£o](modernization_status.md)
- [Schema do Banco de Dados](database_schema.md)
- [DocumentaÃ§Ã£o da API](../api/endpoints.md)

---
### ðŸ•°ï¸ HistÃ³rico de AtualizaÃ§Ãµes
| Data | VersÃ£o | Resumo | Autor |
| :--- | :--- | :--- | :--- |
| 18/02/2026 12:00 | 1.2 | RenomeaÃ§Ã£o para 'Arquitetura de MigraÃ§Ã£o' e consolidaÃ§Ã£o de fluxos. | Victor Hugo Manata Pontes |
| 10/02/2026 14:00 | 1.0 | CriaÃ§Ã£o inicial do diagrama de fluxo de migraÃ§Ã£o. | Victor Hugo Manata Pontes |

