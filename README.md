# 🛣️ Análise Analítica de Acidentes em Vias Federais (PRF) - 2025

![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-orange)
![DuckDB](https://img.shields.io/badge/Motor-DuckDB-yellow)
![SQL](https://img.shields.io/badge/Linguagem-SQL-blue)

## 📌 Visão Geral
Este projeto realiza uma análise aprofundada dos microdados de acidentes de trânsito ocorridos em rodovias federais brasileiras no ano de **2025**, utilizando os dados abertos da **Polícia Rodoviária Federal (PRF)**.

O objetivo é extrair insights críticos sobre a segurança viária, calcular KPIs de severidade (como taxas de acidentes fatais) e mapear a distribuição espaço-temporal das ocorrências, com recortes regionais granulares.

## 🛠️ Arquitetura e Stack Tecnológico
O processamento de dados é totalmente ancorado no **[DuckDB](https://duckdb.org/)**, escolhido por sua alta performance em cargas de trabalho OLAP e processamento *in-memory* / *out-of-core*. 

Vantagens da abordagem utilizada:
* **Ingestão Direta:** Leitura otimizada do `.csv` bruto (com *encoding latin-1*) utilizando `read_csv_auto`, sem necessidade de pré-processamento pesado em Python.
* **SQL Analítico Avançado:** Uso de agregações com a cláusula `FILTER (WHERE ...)` para cálculo de KPIs em uma única passagem, além de formatação dinâmica de *strings* e *floats* direto no motor de banco de dados.

## 📊 Principais Eixos de Análise Implementados

O pipeline SQL atual responde a perguntas críticas de negócio através das seguintes dimensões:

1. **Métricas de Letalidade:** Cálculo absoluto de óbitos e a **Taxa de Acidentes Fatais** (percentual de acidentes que resultam em pelo menos uma morte).
2. **Análise Espacial (Nacional e Regional):** 
   - Ranking de acidentalidade por Estado (UF).
   - *Deep dive* regional focado no estado de **Pernambuco**, isolando municípios críticos da Região Metropolitana (Recife, Olinda, Igarassu e Paulista).
3. **Fatores Contribuintes:** Agrupamento e ranqueamento das principais causas de acidentes pela sua taxa de letalidade.
4. **Sazonalidade:** Distribuição de ocorrências e gravidade consolidada mês a mês.

## 📂 Estrutura de Diretórios

```text
├── dados_brutos/
│   └── acidentes2025.csv      # Base original da PRF (não versionada devido ao tamanho)
├── queries/
│   ├── 01_ingestion.sql       # Criação da tabela e leitura do CSV
│   ├── 02_exploratory.sql     # Filtros regionais e exploração inicial
│   └── 03_aggregations.sql    # Cálculo de KPIs (Taxa de letalidade por UF, Causa, Mês)
└── README.md
