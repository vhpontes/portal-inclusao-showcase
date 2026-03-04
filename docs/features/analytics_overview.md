# AnÃ¡lise do Dashboard e Analytics

## VisÃ£o Geral
A Ã¡rea de Dashboard do Portal InclusÃ£o Ã© composta por dois mÃ³dulos principais no frontend e um controlador dedicado no backend.

### 1. Dashboard Principal (AdminDashboard.vue)
O painel de entrada administrativo fornece uma visÃ£o operacional rÃ¡pida.

**Funcionalidades:**
- **KPIs (Indicadores Chave):**
  - Total de BeneficiÃ¡rios
  - Novos Cadastros (Hoje)
  - SolicitaÃ§Ãµes Aguardando AnÃ¡lise
- **GrÃ¡ficos BÃ¡sicos:**
  - Cadastros por MÃªs (HistÃ³rico de 6 meses)
  - Top 5 DeficiÃªncias (DistribuiÃ§Ã£o)
- **Operacional:**
  - Lista de SolicitaÃ§Ãµes Pendentes (Ãšltimas)
  - Atalhos RÃ¡pidos: UsuÃ¡rios, RelatÃ³rios, MigraÃ§Ã£o, HistÃ³rico, ConfiguraÃ§Ãµes, BI.

**Dados:** Consome `/api/dashboard/stats` + `/api/solicitacoes`.

### 2. Business Intelligence (AdminAnalytics.vue)
Um painel estratÃ©gico avanÃ§ado focado em anÃ¡lise demogrÃ¡fica e de gestÃ£o.

**Funcionalidades:**
- **KPIs EstratÃ©gicos:** Censo Total, Fluxo de Auditoria (7 dias), ProjeÃ§Ã£o de RenovaÃ§Ãµes.
- **GrÃ¡ficos AvanÃ§ados:**
  - Fluxo de Atividade (Linha com gradiente SVG)
  - Cronograma de RenovaÃ§Ãµes (PrÃ³ximos 12 meses)
  - DistribuiÃ§Ã£o EtÃ¡ria (GrÃ¡fico de Donut SVG interativo)
  - Top 5 Bairros (Barras)
- **GeolocalizaÃ§Ã£o:**
  - Mapa de Calor GeogrÃ¡fico (HeatMap) visualizando a densidade de beneficiÃ¡rios por bairro.
  - Componentes: `HeatMapInline.vue`, `HeatMapModal.vue`.

**Dados:** Consome `/api/dashboard/analytics` + `/api/dashboard/geostats`.

---

## Estrutura do Backend (API)

### Roteamento (`api/index.php`)
As rotas estÃ£o mapeadas corretamente para o `DashboardController`:
- `GET /api/dashboard/stats` -> `getStats()`
- `GET /api/dashboard/analytics` -> `getAdvancedStats()`
- `GET /api/dashboard/geostats` -> `getGeostats()`

### Controlador (`DashboardController.php`)

#### MÃ©todo `getStats()`
ResponsÃ¡vel pelos dados do dashboard operacional.
- **Queries:** Contagens simples (COUNT) para KPIs.
- **HistÃ³rico:** Agrupamento por mÃªs (`DATE_FORMAT`).
- **Ponto de AtenÃ§Ã£o:** A lÃ³gica de "Top DeficiÃªncias" recupera todos os beneficiÃ¡rios e processa o JSON no PHP. Isso pode se tornar lento com o crescimento da base de dados. Sugere-se normalizaÃ§Ã£o futura ou cache.

#### MÃ©todo `getAdvancedStats()`
ResponsÃ¡vel pelos dados do BI.
- **Censo Bairros:** Agrupamento SQL.
- **Censo Idades:** Usa `TIMESTAMPDIFF` no SQL para classificar faixas etÃ¡rias (Eficiente).
- **ProjeÃ§Ã£o RenovaÃ§Ãµes:** Calcula datas futuras (+5 anos) para prever carga de trabalho.

#### MÃ©todo `getGeostats()`
- Retorna dados brutos de contagem por bairro para plotagem no mapa.

---

## Componentes Envolvidos

| Componente | Tipo | FunÃ§Ã£o |
|------------|------|--------|
| `AdminDashboard.vue` | View | Entrada administrativa, visÃ£o operacional. |
| `AdminAnalytics.vue` | View | VisÃ£o estratÃ©gica, grÃ¡ficos complexos. |
| `HeatMapInline.vue` | Component | VisualizaÃ§Ã£o de mapa simplificada. |
| `HeatMapModal.vue` | Component | VisualizaÃ§Ã£o de mapa expandida. |
| `DashboardController.php` | Controller | LÃ³gica de negÃ³cios e agregaÃ§Ã£o de dados. |

## ConclusÃ£o
A arquitetura estÃ¡ bem segmentada entre "Operacional" (Dashboard) e "EstratÃ©gico" (Analytics). O frontend utiliza grÃ¡ficos SVG customizados (sem bibliotecas pesadas de charts), o que garante alta performance. O backend Ã© eficiente, com ressalva apenas para o processamento de JSON das deficiÃªncias.

