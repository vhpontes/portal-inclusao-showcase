# Mapeador Visual de Dados - AdminMapper.vue

## VisÃ£o Geral

O **Mapeador Visual** Ã© uma ferramenta interativa que permite criar mapeamentos entre tabelas de bancos de dados diferentes atravÃ©s de uma interface drag-and-drop. Ã‰ especialmente Ãºtil para configurar migraÃ§Ãµes de dados complexas.

## Funcionalidades

### 1. VisualizaÃ§Ã£o de Tabelas

```mermaid
graph LR
    Sidebar[Sidebar de Tabelas] --> Source[Tabelas de Origem<br/>Azul]
    Sidebar --> Target[Tabelas de Destino<br/>Verde]
    
    Source -.->|Drag| Canvas[Canvas Principal]
    Target -.->|Drag| Canvas
    
    Canvas --> Node1[TableNode<br/>legado_cadastro]
    Canvas --> Node2[TableNode<br/>beneficiarios]
```

**CaracterÃ­sticas:**
- **Sidebar**: Lista todas as tabelas disponÃ­veis (Auto-filtragem de prefixos)
- **Origem (Azul)**: Exibe **apenas** tabelas iniciadas com `legado_` para facilitar a identificaÃ§Ã£o.
- **Destino (Verde)**: Tabelas do banco atual
- **Canvas**: Ãrea de trabalho com zoom e pan

### 2. Drag & Drop de Tabelas

**Como usar:**
1. Arraste uma tabela da sidebar para o canvas
2. O sistema busca automaticamente o schema da tabela
3. A tabela aparece como um nÃ³ com todos os campos listados

**Exemplo de NÃ³:**
```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚ legado_cadastro         â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚ â—‹ Id (int)              â”‚
â”‚ â—‹ Nome (varchar)        â”‚
â”‚ â—‹ CPF (text)            â”‚
â”‚ â—‹ Nascimento (date)     â”‚
â”‚ â—‹ Mae (varchar)         â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

### 3. CriaÃ§Ã£o de ConexÃµes

**Conectar Campos:**
1. Clique no cÃ­rculo de um campo de origem (azul)
2. Arraste atÃ© o cÃ­rculo de um campo de destino (verde)
3. Uma linha conecta os dois campos

**Exemplo de Mapeamento:**
```mermaid
graph LR
    subgraph Origem
        O1[Nome]
        O2[CPF]
        O3[Nascimento]
    end
    
    subgraph Destino
        D1[nome]
        D2[cpf]
        D3[data_nascimento]
    end
    
    O1 -->|nome| D1
    O2 -->|cpf| D2
    O3 -->|data_nascimento| D3
    
    style O1 fill:#e1f5ff
    style O2 fill:#e1f5ff
    style O3 fill:#e1f5ff
    style D1 fill:#e1ffe1
    style D2 fill:#e1ffe1
    style D3 fill:#e1ffe1
```

### 4. ExportaÃ§Ã£o e ImportaÃ§Ã£o

#### Exportar JSON
Salva o mapeamento completo incluindo:
- PosiÃ§Ãµes dos nÃ³s no canvas
- Todas as conexÃµes entre campos
- Metadados (data, versÃ£o)

**Formato do Arquivo:**
```json
{
  "metadata": {
    "date": "2026-02-09T19:30:00.000Z",
    "version": "1.0"
  },
  "nodes": [
    {
      "id": "source-legado_cadastro-1707504000",
      "type": "table-node",
      "position": { "x": 100, "y": 100 },
      "data": {
        "label": "legado_cadastro",
        "columns": [
          { "name": "Id", "type": "int(11)" },
          { "name": "Nome", "type": "varchar(255)" }
        ],
        "type": "source"
      }
    }
  ],
  "edges": [
    {
      "id": "edge-1",
      "source": "source-legado_cadastro-1707504000",
      "target": "target-beneficiarios-1707504010",
      "sourceHandle": "src-legado_cadastro-Nome",
      "targetHandle": "tgt-beneficiarios-nome"
    }
  ],
  "mapping": [
    {
      "source": { "table": "legado_cadastro", "field": "Nome" },
      "target": { "table": "beneficiarios", "field": "nome" }
    }
  ]
}
```

#### Importar JSON
Restaura um mapeamento salvo anteriormente, recriando:
- Todos os nÃ³s nas posiÃ§Ãµes originais
- Todas as conexÃµes entre campos

### 5. Salvar Mapeamento

O botÃ£o **"Salvar Mapeamento"** persiste a configuraÃ§Ã£o no backend:
- Arquivo: `migration_config.json`
- Formato: Estrutura de mapeamento por par de tabelas
- Uso: Aplicado automaticamente em migraÃ§Ãµes futuras

## Interface do UsuÃ¡rio

### Layout

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚ Mapeamento Visual                                      â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚          â”‚                                             â”‚
â”‚ Tabelas  â”‚          Canvas Principal                   â”‚
â”‚          â”‚                                             â”‚
â”‚ â”Œâ”€â”€â”€â”€â”€â”€â” â”‚  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”         â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”         â”‚
â”‚ â”‚Origemâ”‚ â”‚  â”‚ Tabela 1 â”‚â”€â”€â”€â”€â”€â”€â”€â”€â–¶â”‚ Tabela 2 â”‚         â”‚
â”‚ â”œâ”€â”€â”€â”€â”€â”€â”¤ â”‚  â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜         â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜         â”‚
â”‚ â”‚ T1   â”‚ â”‚                                             â”‚
â”‚ â”‚ T2   â”‚ â”‚                                             â”‚
â”‚ â”‚ T3   â”‚ â”‚                                             â”‚
â”‚ â””â”€â”€â”€â”€â”€â”€â”˜ â”‚                                             â”‚
â”‚          â”‚                                             â”‚
â”‚ â”Œâ”€â”€â”€â”€â”€â”€â” â”‚                                             â”‚
â”‚ â”‚Dest. â”‚ â”‚                                             â”‚
â”‚ â”œâ”€â”€â”€â”€â”€â”€â”¤ â”‚                                             â”‚
â”‚ â”‚ T4   â”‚ â”‚  [Salvar] [Exportar] [Importar]            â”‚
â”‚ â”‚ T5   â”‚ â”‚                                             â”‚
â”‚ â””â”€â”€â”€â”€â”€â”€â”˜ â”‚                                             â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

### Controles

| BotÃ£o | AÃ§Ã£o |
|-------|------|
| **Salvar Mapeamento** | Persiste configuraÃ§Ã£o no backend |
| **Exportar JSON** | Download do arquivo de configuraÃ§Ã£o |
| **Importar JSON** | Upload de configuraÃ§Ã£o salva |
| **Zoom +/-** | Controles de zoom do VueFlow |
| **Fit View** | Ajusta visualizaÃ§Ã£o para mostrar todos os nÃ³s |

## Tecnologias Utilizadas

- **@vue-flow/core**: Biblioteca para diagramas interativos
- **@vue-flow/background**: Grid de fundo
- **@vue-flow/controls**: Controles de zoom e navegaÃ§Ã£o
- **Fetch API**: ComunicaÃ§Ã£o com backend

## Casos de Uso

### Caso 1: MigraÃ§Ã£o Simples

```
Objetivo: Migrar tabela legado_cadastro â†’ beneficiarios

Passos:
1. Arrastar "legado_cadastro" para o canvas
2. Arrastar "beneficiarios" para o canvas
3. Conectar campos:
   - Id â†’ legado_id
   - Nome â†’ nome
   - CPF â†’ cpf
   - Nascimento â†’ data_nascimento
4. Clicar em "Salvar Mapeamento"
5. ConfiguraÃ§Ã£o aplicada automaticamente em migraÃ§Ãµes
```

### Caso 2: Mapeamento Complexo

```
Objetivo: Mapear mÃºltiplas tabelas com transformaÃ§Ãµes

Passos:
1. Adicionar tabelas de origem: legado_cadastro, legado_arquivos
2. Adicionar tabelas de destino: beneficiarios, documentos
3. Criar conexÃµes entre campos relacionados
4. Exportar JSON para backup
5. Salvar mapeamento no sistema
```

## IntegraÃ§Ã£o com MigraÃ§Ã£o

O mapeamento salvo Ã© utilizado automaticamente pelo `MigrationService`:

```php
// Backend: MigrationService.php
$legacyMap = [
    'nome' => 'Nome',
    'cpf' => 'CPF',
    // ...
];

// Merge com mapeamento customizado
if (!empty($config['field_map'])) {
    $legacyMap = array_merge($legacyMap, $config['field_map']);
}
```

## LimitaÃ§Ãµes Conhecidas

- NÃ£o valida tipos de dados incompatÃ­veis
- NÃ£o suporta transformaÃ§Ãµes complexas (ex: concatenaÃ§Ã£o)
- Mapeamento 1:1 apenas (um campo origem â†’ um campo destino)

## PrÃ³ximos Passos

- [DocumentaÃ§Ã£o de MigraÃ§Ã£o](../architecture/migration_diagram.md)
- [EspecificaÃ§Ã£o da API](../api/spec.yaml)

---
### ðŸ•°ï¸ HistÃ³rico de AtualizaÃ§Ãµes
| Data | VersÃ£o | Resumo | Autor |
| :--- | :--- | :--- | :--- |
| 18/02/2026 11:45 | 1.2 | SincronizaÃ§Ã£o estrutural e organizaÃ§Ã£o de metadados. | Victor Hugo Manata Pontes |
| 18/02/2026 09:00 | 1.1 | Melhorias de performance no processamento de mapeamento. | Victor Hugo Manata Pontes |
| 14/02/2026 16:00 | 0.8 | RevisÃ£o visual dos nÃ³s de tabela. | Victor Hugo Manata Pontes |
| 09/02/2026 18:00 | 1.0 | DocumentaÃ§Ã£o inicial da lÃ³gica do Mapeador Visual. | Victor Hugo Manata Pontes |

