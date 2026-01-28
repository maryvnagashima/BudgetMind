# 🏗️ Arquitetura do BudgetMind

Este documento descreve a arquitetura técnica do **BudgetMind**, incluindo fluxo de dados, componentes principais, integrações e decisões de desenho do sistema.

---

## 🔍 Visão Geral

O BudgetMind é uma aplicação SaaS que:

- Coleta dados de múltiplas plataformas de mídia paga.
- Armazena e normaliza esses dados em um data warehouse.
- Expõe uma interface web para análise de performance e recomendações de budget com IA.

Fluxo macro:

```text
APIs de Mídia  →  Ingestão (Cloud Functions)  →  BigQuery (modelo de dados)
                                                   ↓
                                              Firestore (estado/app)
                                                   ↓
                                        Frontend (React + TypeScript)
                                                   ↓
                                      IA (Gemini) para recomendações
