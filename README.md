# 💰 BudgetMind – SaaS de Otimização de Budget de Mídia

O **BudgetMind** é uma plataforma SaaS que consolida dados de mídia paga de múltiplos canais e usa IA para sugerir a melhor redistribuição de investimento, ajudando times de marketing a aumentarem ROAS e reduzirem o tempo gasto em planilhas.

---

## 🚨 Problema que o BudgetMind resolve

Times de performance e agências enfrentam três dores recorrentes:

- Gastam **1–3 horas por dia** exportando relatórios de Google Ads, Meta, Shopee, etc. e consolidando tudo em Excel.
- Tomam decisões de budget de forma **reativa e intuitiva**, sem visão clara de qual canal ou região realmente entrega mais resultado.
- Têm dificuldade de enxergar **saturação geográfica e por canal**, desperdiçando verba em campanhas que já bateram teto de escala.

O resultado é um mix de mídia ineficiente, tempo desperdiçado e oportunidades de otimização que passam despercebidas.

---

## ✅ Visão Geral da Solução

O BudgetMind foi criado para ser o **copiloto de mídia paga**:

- Consolida dados de 5+ plataformas de mídia em um único painel.
- Normaliza métricas (ROAS, CPA, CAC, LTV) para comparação justa entre canais.
- Analisa performance por **canal, campanha, criativo e geografia**.
- Usa IA (Gemini) para sugerir redistribuições de budget com base em regras de negócio e histórico.
- Oferece uma **central de alertas inteligentes** para identificar problemas e oportunidades rapidamente.

---

## 🧠 Principais Funcionalidades

### 1. Consolidação Multicanal Automática

- Conecta com:
  - Google Ads API
  - Meta Ads API
  - Shopee Ads
  - (Opcional) TikTok Ads
  - Google Analytics 4

- Faz ingestão periódica dos dados (via jobs agendados em Cloud Functions).
- Cria uma camada de dados unificada com métricas padronizadas:
  - Impressões, cliques, custo, conversões, receita, ROAS, CPA, CTR, CVR.

---

### 2. Análise Geográfica (País → Estado → Cidade)

- Mapa interativo mostrando performance por região.
- Drill-down de Brasil → Estado → Cidade.
- Identificação de:
  - Regiões saturadas (CPA alto, ROAS caindo).
  - Regiões subexploradas com bom potencial (CPA baixo, ROAS alto).
- Sugestões do tipo:
  - “Reduzir 20% do investimento em SP e realocar para MG, onde o CPA é 35% menor.”

---

### 3. Central de Alertas Inteligentes

Três tipos de alertas:

- **Críticos (vermelho):**
  - CPA > 30% acima da meta.
  - ROAS abaixo do limite definido.
  - Queda brusca de conversões.

- **Oportunidades (verde):**
  - Canal com ROAS muito acima da média sustentado por X dias.
  - Campanhas com CPA muito abaixo da meta (sinal de espaço para escala).

- **Sistema (azul):**
  - API fora do ar.
  - Falha em job de ingestão.
  - Problema de autenticação.

Os alertas aparecem na interface e podem ser integrados a e-mail/Slack em versões futuras.

---

### 4. Recomendações de Budget com IA

- O BudgetMind usa **Gemini** para analisar:
  - Histórico de performance.
  - Limites de budget.
  - Metas de ROAS e CPA.
  - Restrições de negócio (mínimo/máximo por canal).

- Gera sugestões do tipo:
  - “Mover R$ 15.000 de Google Search para Meta Reels pode aumentar o ROAS total esperado em 18%.”
  - “Reduzir 10% em Shopee e realocar para campanhas de marca em Google Ads.”

- As recomendações **não são aplicadas automaticamente**: a decisão final é humana — o sistema atua como assistente inteligente.

---

### 5. Simulador de Cenários (Roadmap)

Funcionalidade planejada para próximas versões:

- Permitir ao usuário testar:
  - “E se eu aumentar em 20% o budget de Meta?”
  - “E se eu cortar 50% de campanhas com ROAS < 2?”
- Exibir impacto esperado em:
  - ROAS consolidado.
  - Receita total.
  - Distribuição de CPA por canal.

---

## 🏗️ Arquitetura (Visão Técnica)

### Visão Geral

```text
APIs de Mídia  →  Cloud Functions (ingestão)  →  BigQuery (camada de dados)
                                                 ↓
                                           Firestore (estado/usuário)
                                                 ↓
                                      Frontend React (BudgetMind UI)
                                                 ↓
                                     Gemini (recomendações de budget)
