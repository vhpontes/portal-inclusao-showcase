# Análise do Dashboard e Analytics

## Visão Geral
A área de Dashboard do Portal Inclusão é composta por dois módulos principais no frontend e um controlador dedicado no backend.

### 1. Dashboard Principal (AdminDashboard.vue)
O painel de entrada administrativo fornece uma visão operacional rápida.

**Funcionalidades:**
- **KPIs (Indicadores Chave):**
  - Total de Beneficiários
  - Novos Cadastros (Hoje)
  - Solicitações Aguardando Análise
- **Gráficos Básicos:**
  - Cadastros por Mês (Histórico de 6 meses)
  - Top 5 Deficiências (Distribuição)
- **Operacional:**
  - Lista de Solicitações Pendentes (Últimas)
  - Atalhos Rápidos: Usuários, Relatórios, Migração, Histórico, Configurações, BI.

**Dados:** Consome `/api/dashboard/stats` + `/api/solicitacoes`.

### 2. Business Intelligence (AdminAnalytics.vue)
Um painel estratégico avançado focado em análise demográfica e de gestão.

**Funcionalidades:**
- **KPIs Estratégicos:** Censo Total, Fluxo de Auditoria (7 dias), Projeção de Renovações.
- **Gráficos Avançados:**
  - Fluxo de Atividade (Linha com gradiente SVG)
  - Cronograma de Renovações (Próximos 12 meses)
  - Distribuição Etária (Gráfico de Donut SVG interativo)
  - Top 5 Bairros (Barras)
- **Geolocalização:**
  - Mapa de Calor Geográfico (HeatMap) visualizando a densidade de beneficiários por bairro.
  - Componentes: `HeatMapInline.vue`, `HeatMapModal.vue`.

**Dados:** Consome `/api/dashboard/analytics` + `/api/dashboard/geostats`.

---

## Estrutura do Backend (API)

### Roteamento (`api/index.php`)
As rotas estão mapeadas corretamente para o `DashboardController`:
- `GET /api/dashboard/stats` -> `getStats()`
- `GET /api/dashboard/analytics` -> `getAdvancedStats()`
- `GET /api/dashboard/geostats` -> `getGeostats()`

### Controlador (`DashboardController.php`)

#### Método `getStats()`
Responsável pelos dados do dashboard operacional.
- **Queries:** Contagens simples (COUNT) para KPIs.
- **Histórico:** Agrupamento por mês (`DATE_FORMAT`).
- **Ponto de Atenção:** A lógica de "Top Deficiências" recupera todos os beneficiários e processa o JSON no PHP. Isso pode se tornar lento com o crescimento da base de dados. Sugere-se normalização futura ou cache.

#### Método `getAdvancedStats()`
Responsável pelos dados do BI.
- **Censo Bairros:** Agrupamento SQL.
- **Censo Idades:** Usa `TIMESTAMPDIFF` no SQL para classificar faixas etárias (Eficiente).
- **Projeção Renovações:** Calcula datas futuras (+5 anos) para prever carga de trabalho.

#### Método `getGeostats()`
- Retorna dados brutos de contagem por bairro para plotagem no mapa.

---

## Componentes Envolvidos

| Componente | Tipo | Função |
|------------|------|--------|
| `AdminDashboard.vue` | View | Entrada administrativa, visão operacional. |
| `AdminAnalytics.vue` | View | Visão estratégica, gráficos complexos. |
| `HeatMapInline.vue` | Component | Visualização de mapa simplificada. |
| `HeatMapModal.vue` | Component | Visualização de mapa expandida. |
| `DashboardController.php` | Controller | Lógica de negócios e agregação de dados. |

## Conclusão
A arquitetura está bem segmentada entre "Operacional" (Dashboard) e "Estratégico" (Analytics). O frontend utiliza gráficos SVG customizados (sem bibliotecas pesadas de charts), o que garante alta performance. O backend é eficiente, com ressalva apenas para o processamento de JSON das deficiências.
