# 🎬 CineData Analytics - Agente Inteligente Text-to-SQL

Este projeto consiste no desenvolvimento de um agente inteligente capaz de realizar consultas e análises exploratórias em tempo real sobre o catálogo de filmes da **CineData Analytics**.

O objetivo principal é democratizar o acesso aos dados da camada Gold, permitindo que usuários não técnicos façam perguntas em linguagem natural sem a necessidade de escrever consultas SQL.

---

### 🧰 Stack Técnica
* **Linguagem:** Python
* **LLM / Provider:** OpenRouter API (Modelos Gratuitos: Gemini 2.0 / Qwen 2.5 Coder)
* **Banco de Dados:** SQLite3 (`cinerocket.db`)
* **Processamento de Dados:** Pandas

---

### 📐 Estrutura do Data Lakehouse (Modelagem Dimensional)
O banco de dados é composto por 10 tabelas conectadas por chave primária/estrangeira (`sk_movie_id`, `sk_person_id`, etc.):
* **Dimensões e Fatos Principais:** `dim_movies`, `fact_movies_performance`, `dim_genres`, `dim_people`, `dim_companies`, `dim_reviews`, `movie_reviews`
* **Tabelas Ponte (Bridge):** `bridge_movie_genre`, `bridge_movie_person`, `bridge_movie_company`

---



### 🤖 Arquitetura do Agente e Escolha do Framework

Para a implementação do agente **Text-to-SQL**, adotamos a seguinte estratégia:

* **Framework de Agentes:** **Custom Agent em Python Nativo (SDK OpenAI + OpenRouter API)**
  * **Motivação:** A escolha de um agente customizado em Python nativo garante execução leve e controle preciso das requisições, evitando *overhead* de bibliotecas e otimizando o consumo da cota gratuita da API.
* **Mecanismo de Fallback Automático:**
  * Para contornar limites de taxa (*Rate Limits / HTTP 429*) e indisponibilidade momentânea de provedores gratuitos do OpenRouter, o agente percorre dinamicamente uma lista prioritária de LLMs (`nvidia/nemotron-3.5-lightning:free`, `dots-studio/dots3-note-preview:free`, `apodex/apodex-1.1-mini:free`, `qwen/qwen3.8-27b:free`).
* **Execução Segura:**
  * O agente possui *guardrails* no prompt do sistema para garantir que **apenas consultas de leitura (`SELECT`)** sejam geradas e executadas com segurança sobre a camada Gold (`cinerocket.db`).

---

