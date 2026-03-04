# IntegraÃ§Ã£o Backend IA

O Portal de InclusÃ£o agora persiste todos os resultados detalhados das anÃ¡lises de InteligÃªncia Artificial no banco de dados.

## Estrutura de Dados
Os dados sÃ£o armazenados na coluna `validacoes_ai` da tabela `solicitacoes` em formato JSON.

### Exemplo de Payload Armazenado
```json
{
  "foto_3x4": {
    "valid": false,
    "details": {
      "hasFace": true,
      "centered": false,
      "sharp": true,
      "single": true,
      "framing": false,
      "noAccessories": true
    }
  },
  "laudo_medico": {
    "valid": true,
    "details": {
      "hasCID": true,
      "nameMatch": true,
      "cids": ["F84.0"],
      "crm": {
        "uf": "SP",
        "numero": "195959",
        "found": true
      },
      "doctor": {
        "valid": true,
        "nome": "DR. MATEUS KENJI CHRISTO MIYAHIRA",
        "situacao": "Regular",
        "especialidade": "ClÃ­nica MÃ©dica"
      },
      "rawText": "LAUDO MÃ‰DICO..."
    },
    "rawText": "LAUDO MÃ‰DICO..."
  }
}
```

## Fluxo de GravaÃ§Ã£o
1. **Frontend**: O usuÃ¡rio preenche o cadastro e a IA roda localmente (Browser).
2. **Submit**: Ao clicar em "Confirmar", o objeto `formData.validacoes` Ã© serializado (`JSON.stringify`) e anexado ao `FormData` com a chave `validacoes`.
3. **Backend (Controller)**: O `RequestController.php` recebe o POST, decodifica o JSON (se necessÃ¡rio) e o repassa ao Model.
4. **Backend (Model)**: O `Request.php` insere os dados brutos na coluna `validacoes_ai` no momento do `INSERT`.

## Consultas e RelatÃ³rios
Como os dados estÃ£o em JSON, Ã© possÃ­vel realizar consultas analÃ­ticas diretamente no banco de dados (MySQL 5.7+ / MariaDB 10.2+):

```sql
-- Listar laudos com CRM de SP
SELECT id, protocolo, JSON_EXTRACT(validacoes_ai, '$.laudo_medico.details.crm.uf') as uf 
FROM solicitacoes 
WHERE JSON_EXTRACT(validacoes_ai, '$.laudo_medico.details.crm.uf') = 'SP';
```

