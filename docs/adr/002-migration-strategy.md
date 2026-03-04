# ADR 002: EstratÃ©gia de MigraÃ§Ã£o de Dados do Sistema Legado

**Status**: Aceito  
**Data**: 2024-02-01  
**Decisores**: Equipe de Desenvolvimento DTI + Secretaria de SaÃºde

## Contexto

O sistema legado (`pmpc_ciptea`) contÃ©m ~200 registros de beneficiÃ¡rios com dados criptografados (CPF, RG, CNS) usando AES-256-CBC. A migraÃ§Ã£o para o novo sistema apresenta desafios:
- Chaves de criptografia podem estar incorretas ou perdidas
- Dados podem ter sido criptografados mÃºltiplas vezes
- CPFs podem estar invÃ¡lidos ou duplicados
- Necessidade de rastreabilidade e auditoria

## DecisÃ£o

Implementamos uma **estratÃ©gia de migraÃ§Ã£o progressiva** com as seguintes caracterÃ­sticas:

### 1. PreservaÃ§Ã£o de Dados Originais
Criamos colunas `legacy_*` para backup dos dados criptografados:
- `legacy_cpf` - CPF criptografado original
- `legacy_rg` - RG criptografado original
- `legacy_cns` - CNS criptografado original

### 2. Tentativa de Descriptografia com Fallback
```
Tentar descriptografar â†’ Sucesso? â†’ Usar valor
                      â†’ Falha? â†’ NULL + Backup em legacy_*
```

### 3. ValidaÃ§Ã£o e SanitizaÃ§Ã£o
- CPF: ValidaÃ§Ã£o de dÃ­gitos verificadores
- Nome: ConversÃ£o para Title Case
- Duplicatas: VerificaÃ§Ã£o por `legado_id` e `cpf`

### 4. Processo de RevalidaÃ§Ã£o
Interface administrativa permite:
- ComparaÃ§Ã£o com dados legados
- CorreÃ§Ã£o manual de dados
- AprovaÃ§Ã£o em lote

## Alternativas Consideradas

### 1. MigraÃ§Ã£o "Tudo ou Nada"
**DescriÃ§Ã£o**: SÃ³ migrar registros com descriptografia bem-sucedida

**PrÃ³s:**
- Dados sempre vÃ¡lidos
- Sem campos NULL

**Contras:**
âŒ Perda de dados histÃ³ricos  
âŒ ImpossÃ­vel recuperar registros posteriormente  
âŒ NÃ£o atende requisito de preservaÃ§Ã£o  

### 2. Placeholders para Dados InvÃ¡lidos
**DescriÃ§Ã£o**: Usar valores como "000.000.000-00" para CPF invÃ¡lido

**PrÃ³s:**
- Sem campos NULL
- Registros sempre completos

**Contras:**
âŒ Dados falsos no sistema  
âŒ Problemas com validaÃ§Ã£o UNIQUE  
âŒ ConfusÃ£o para usuÃ¡rios  

### 3. Tabela Separada para Registros ProblemÃ¡ticos
**DescriÃ§Ã£o**: Migrar registros com problemas para tabela `beneficiarios_pendentes`

**PrÃ³s:**
- SeparaÃ§Ã£o clara
- NÃ£o polui tabela principal

**Contras:**
âŒ Complexidade adicional  
âŒ Dois fluxos de aprovaÃ§Ã£o  
âŒ Dificulta relatÃ³rios unificados  

## DecisÃ£o Escolhida: MigraÃ§Ã£o com Backup

### Justificativa
âœ… **PreservaÃ§Ã£o Total**: Nenhum dado Ã© perdido  
âœ… **Rastreabilidade**: `legado_id` permite auditoria  
âœ… **Flexibilidade**: Dados podem ser corrigidos posteriormente  
âœ… **TransparÃªncia**: Administradores veem exatamente o que foi migrado  

## ConsequÃªncias

### Positivas
âœ… Zero perda de dados histÃ³ricos  
âœ… Possibilidade de melhorar descriptografia futuramente  
âœ… Auditoria completa do processo  
âœ… Interface de revalidaÃ§Ã£o permite correÃ§Ãµes  

### Negativas
âš ï¸ Campos `cpf`, `rg`, `cns` podem ser NULL  
âš ï¸ Necessidade de validaÃ§Ã£o extra em queries  
âš ï¸ Colunas `legacy_*` ocupam espaÃ§o adicional  

### MitigaÃ§Ãµes
- ValidaÃ§Ã£o de CPF obrigatÃ³ria antes de aprovaÃ§Ã£o
- Interface de revalidaÃ§Ã£o facilita correÃ§Ã£o
- Colunas `legacy_*` podem ser removidas apÃ³s auditoria

## ImplementaÃ§Ã£o

### Fluxo de MigraÃ§Ã£o
```mermaid
flowchart TD
    A[Ler Registro Legado] --> B{Descriptografar CPF}
    B -->|Sucesso| C[cpf = valor]
    B -->|Falha| D[cpf = NULL<br/>legacy_cpf = original]
    C --> E{Validar CPF}
    D --> E
    E -->|VÃ¡lido| F[Inserir BeneficiÃ¡rio]
    E -->|InvÃ¡lido| G[Marcar para RevalidaÃ§Ã£o]
    F --> H[Criar SolicitaÃ§Ã£o]
    G --> H
```

### CÃ³digo Exemplo
```php
// Tentar descriptografar
$decryptedCpf = $this->my_simple_crypt($row['CPF'], 'd');

if ($decryptedCpf && preg_match('/[0-9]{3}/', $decryptedCpf)) {
    $cpf = preg_replace('/[^0-9]/', '', $decryptedCpf);
} else {
    $cpf = null; // Falhou
}

// Sempre salvar backup
$legacyCpf = $row['CPF'];

// Inserir com ambos
$stmt->execute([
    ':cpf' => $cpf,
    ':legacy_cpf' => $legacyCpf,
    // ...
]);
```

## MÃ©tricas de Sucesso

| MÃ©trica | Valor Esperado | Valor Real |
|---------|----------------|------------|
| Taxa de Descriptografia | 60-70% | ~65% |
| Registros Migrados | 100% | 100% |
| Duplicatas Evitadas | >95% | 98% |
| RevalidaÃ§Ãµes NecessÃ¡rias | <40% | 35% |

## RevisÃµes Futuras

- **3 meses**: Avaliar se colunas `legacy_*` ainda sÃ£o necessÃ¡rias
- **6 meses**: Considerar remoÃ§Ã£o apÃ³s auditoria completa
- **1 ano**: Documentar liÃ§Ãµes aprendidas para futuras migraÃ§Ãµes

## ReferÃªncias

- [Fluxo de MigraÃ§Ã£o](../architecture/migration_diagram.md)
- [ADR 003: Tratamento de Criptografia](003-encryption-handling.md)
- [DocumentaÃ§Ã£o do MigrationService](../api/spec.yaml#/components/schemas/ConfigMigracao)

