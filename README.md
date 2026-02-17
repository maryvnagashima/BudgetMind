# 💰 BudgetMind – SaaS de Otimização de Budget de Mídia

O **BudgetMind** é uma plataforma SaaS que consolida dados de mídia paga de múltiplos canais e usa IA para sugerir a melhor redistribuição de investimento, ajudando times de marketing a aumentarem ROAS e reduzirem o tempo gasto em planilhas. [peliqan](https://peliqan.io/blog/etl-best-practices/)

***

## 🚨 Problema que o BudgetMind resolve

Times de performance e agências enfrentam três dores recorrentes:

- Gastam 1–3 horas por dia exportando relatórios de Google Ads, Meta, Shopee etc. e consolidando tudo em Excel. [peliqan](https://peliqan.io/blog/etl-best-practices/)
- Tomam decisões de budget de forma reativa e intuitiva, sem visão clara de qual canal ou região realmente entrega mais resultado. [scaler](https://www.scaler.com/blog/20-best-data-analyst-projects/)
- Têm dificuldade de enxergar saturação geográfica e por canal, desperdiçando verba em campanhas que já bateram teto de escala.

O resultado é um mix de mídia ineficiente, tempo desperdiçado e oportunidades de otimização que passam despercebidas.

***

## ✅ Visão geral da solução

O **BudgetMind** foi criado para ser o **copiloto de mídia paga**:

- Consolida dados de 5+ plataformas de mídia em um único painel. [refontelearning](https://www.refontelearning.com/blog/data-engineering-in-2026-trends-tools-and-how-to-thrive)
- Normaliza métricas (ROAS, CPA, CAC, LTV) para comparação justa entre canais.  
- Analisa performance por canal, campanha, criativo e geografia.  
- Usa IA (Gemini) para sugerir redistribuições de budget com base em regras de negócio e histórico. [refontelearning](https://www.refontelearning.com/blog/data-engineering-in-2026-trends-tools-and-how-to-thrive)
- Oferece uma central de alertas inteligentes para identificar problemas e oportunidades rapidamente.

***

## 🧠 Principais funcionalidades

### 1. Consolidação multicanal automática

- Conecta com (na demo: dados simulados, na versão comercial: APIs reais):  
  - Google Ads  
  - Meta Ads  
  - Shopee Ads  
  - (Opcional) TikTok Ads  
  - Google Analytics 4 [peliqan](https://peliqan.io/blog/etl-best-practices/)
- Ingestão periódica dos dados (jobs agendados / funções em nuvem).  
- Cria uma camada de dados unificada com métricas padronizadas:  
  - impressões, cliques, custo, conversões, receita, ROAS, CPA, CTR, CVR. [tapdata](https://tapdata.io/articles/mastering-etl-with-sql-server-best-practices-and-tips/)

### 2. Análise geográfica (País → Estado → Cidade)

- Mapa interativo mostrando performance por região.  
- Drill-down de Brasil → Estado → Cidade.  
- Identificação de:  
  - regiões saturadas (CPA alto, ROAS caindo),  
  - regiões subexploradas com bom potencial (CPA baixo, ROAS alto). [scaler](https://www.scaler.com/blog/20-best-data-analyst-projects/)
- Sugestões do tipo:  
  - “Reduzir 20% do investimento em SP e realocar para MG, onde o CPA é 35% menor.”

### 3. Central de alertas inteligentes

Três tipos de alertas:

- **Críticos (vermelho)**  
  - CPA > 30% acima da meta.  
  - ROAS abaixo do limite definido.  
  - Queda brusca de conversões.  
- **Oportunidades (verde)**  
  - Canal com ROAS muito acima da média por X dias.  
  - Campanhas com CPA muito abaixo da meta (sinal de espaço para escala).  
- **Sistema (azul)**  
  - API fora do ar.  
  - Falha em job de ingestão.  
  - Problema de autenticação.

Os alertas aparecem na interface e podem ser integrados a e-mail/Slack em versões futuras.

### 4. Recomendações de budget com IA

O BudgetMind usa **Gemini** para analisar:

- histórico de performance,  
- limites de budget,  
- metas de ROAS e CPA,  
- restrições de negócio (mínimo/máximo por canal). [refontelearning](https://www.refontelearning.com/blog/data-engineering-in-2026-trends-tools-and-how-to-thrive)

Gera sugestões do tipo:

- “Mover R$ 15.000 de Google Search para Meta Reels pode aumentar o ROAS total esperado em 18%.”  
- “Reduzir 10% em Shopee e realocar para campanhas de marca em Google Ads.”

As recomendações não são aplicadas automaticamente: a decisão final é humana — o sistema atua como assistente inteligente.

### 5. Simulador de cenários (Roadmap)

Funcionalidade planejada:

- “E se eu aumentar em 20% o budget de Meta?”  
- “E se eu cortar 50% de campanhas com ROAS < 2?”  
- Exibir impacto esperado em:  
  - ROAS consolidado,  
  - receita total,  
  - distribuição de CPA por canal. [scaler](https://www.scaler.com/blog/20-best-data-analyst-projects/)

***

## 🏗️ Arquitetura (visão técnica)

### Visão geral

```text
APIs / Dados de Mídia
    ↓ (ETL)
Camada de ingestão (Cloud Functions / scripts Python)
    ↓
BigQuery (camada de dados unificada)
    ↓
API / camada de serviço
    ↓
Frontend React (BudgetMind UI)
    ↓
Gemini (recomendações de budget)
```

Essa arquitetura segue boas práticas de projetos de dados modernos: ingestão automatizada, modelagem analítica em DW e camada de visualização orientada ao negócio. [github](https://github.com/itsyashk1406/sql-portfolio-data-warehouse)

***

## 🔄 Jornada do dado (ETL → SQL → BI)

### Extração

- Scripts / funções leem dados de:  
  - plataformas de mídia (ou datasets simulados em `data_sample/` na versão open source),  
  - analytics (sessões, conversões). [peliqan](https://peliqan.io/blog/etl-best-practices/)
- Tratamento básico de erros (retry, logs) e registro de execuções. [reddit](https://www.reddit.com/r/ETL/comments/1oiouqb/how_do_you_handle_your_etl_and_reporting_data/)

### Transformação e modelagem

- Modelo dimensional no DW com foco em marketing de performance: [github](https://github.com/itsyashk1406/sql-portfolio-data-warehouse)
  - `fact_campaign_performance` (impressões, cliques, custo, conversões, receita, ROAS, CPA).  
  - `dim_channel` (Google, Meta, Shopee etc.).  
  - `dim_geo` (país, estado, cidade).  
  - `dim_time` (data, semana, mês).  
- Scripts SQL organizados em camadas (exemplo):  
  - `bronze/` – staging de dados brutos.  
  - `silver/` – dados limpos e normalizados.  
  - `gold/` – tabelas analíticas consumidas pelo front. [github](https://github.com/itsyashk1406/sql-portfolio-data-warehouse)

### Carga & consumo

- Tabelas atualizadas em janelas diárias/horárias, conforme a fonte. [reddit](https://www.reddit.com/r/ETL/comments/1oiouqb/how_do_you_handle_your_etl_and_reporting_data/)
- A camada de API expõe endpoints como:  
  - `/api/kpis`, `/api/regions`, `/api/alerts`, `/api/recommendations`.  
- O frontend consome esses endpoints e monta páginas como `Dashboard`, `RegionalAnalysis`, `SaturationAnalysis`, `AlertsCenter`, `ProductAnalytics`.

***

## 🧱 Stack técnica

- **Backend / Dados**  
  - Cloud Functions / scripts Python para ETL e integração com APIs de mídia. [refontelearning](https://www.refontelearning.com/blog/data-engineering-in-2026-trends-tools-and-how-to-thrive)
  - BigQuery como data warehouse principal.  
  - SQL organizado por camadas (bronze/silver/gold). [github](https://github.com/itsyashk1406/sql-portfolio-data-warehouse)

- **Frontend**  
  - React + TypeScript:  
    - `App.tsx`, `pages/` (`Dashboard`, `AdminDashboard`, `ProductAnalytics`, `RegionalAnalysis`, `SaturationAnalysis`, `AlertsCenter`, `CRM`, `Simulator`, `Settings`, `UserManagement`, `Integrations`, `LandingPage`, `Home`),  
    - `components/`, `context/` (`CampaignContext`, `NotificationContext`), `utils.ts`, `types.ts`.  

- **IA / Recomendações**  
  - Gemini para geração de insights e recomendações de redistribuição de budget.

***

## 📊 Métricas de negócio & impacto esperado

O BudgetMind foi desenhado para suportar hipóteses de impacto como:

- Reduzir em **X%** o tempo gasto semanalmente em consolidação manual de relatórios. [peliqan](https://peliqan.io/blog/etl-best-practices/)
- Aumentar o ROAS consolidado ao realocar budget de canais/regiões saturados para oportunidades. [scaler](https://www.scaler.com/blog/20-best-data-analyst-projects/)
- Ajudar gestores a identificar rapidamente campanhas com potencial de escala ou necessidade de corte.

Na documentação técnica (`docs/data_model.md`), são detalhados exemplos de KPIs e queries usados para alimentar o dashboard.

***

## 🚀 Como rodar (versão de demonstração)

> A versão open source deste repositório utiliza **dados simulados** em `data_sample/` e não expõe chaves reais de API.

### 1. Clonar o repositório

```bash
git clone https://github.com/maryvnagashima/BudgetMind.git
cd BudgetMind
```

### 2. Configurar variáveis de ambiente

Crie um arquivo `.env` com base em `.env.example`:

```env
# Exemplo
GOOGLE_ADS_API_KEY=...
META_API_KEY=...
SHOPEE_API_KEY=...
GEMINI_API_KEY=...
```

Para rodar só a demo com dados simulados, você pode deixar as chaves em branco e usar os datasets de exemplo.

### 3. Subir backend (se aplicável)

```bash
cd backend
# instalar dependências
# rodar scripts ETL de exemplo
```

### 4. Subir frontend

```bash
cd frontend
npm install
npm run dev
```

Acesse o endereço indicado (por exemplo `http://localhost:5173`) para ver o BudgetMind em ação.

***

## ✅ Qualidade, testes e boas práticas

- **ETL**  
  - Checagens básicas de schema e tipos de dados antes de carregar no DW. [github](https://github.com/itsyashk1406/sql-portfolio-data-warehouse)
  - Logs de execução e falhas em jobs agendados.

- **Código**  
  - Estrutura de pastas organizada por responsabilidade (ETL, SQL, API, UI). [github](https://github.com/data-engineering-community/data-engineering-project-template/blob/main/README.md)
  - Versionamento com Git e instruções claras de setup.

- **Limitações conhecidas**  
  - A versão de demonstração usa dados simulados e não cobre todas as integrações da versão comercial.  
  - O simulador de cenários está em roadmap.

***

## 💼 Versão comercial (open core)

Este repositório representa a **versão open core** do BudgetMind, focada em demonstrar:

- arquitetura de dados (ETL + DW + React),  
- uso de GenAI para recomendações,  
- dashboards orientados ao negócio.

A **versão comercial** inclui:

- mais integrações de plataformas,  
- suporte multi-tenant e autenticação avançada,  
- alertas configuráveis por usuário/time,  
- simulador de cenários completo,  
- suporte e onboarding dedicado.
