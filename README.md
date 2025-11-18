# E-commerce End-to-End Data Engineering (Databricks Community)  
**Projeto:** E-COMMERCE + CLICKSTREAM (Olist + Event Simulation)  
**Stack:** Databricks Community Edition, PySpark, Delta Lake, Delta Tables, DBFS, Python

## Visão Geral
Pipeline Medallion (Bronze → Silver → Gold) unindo dados transacionais (Olist) + eventos simulados (clickstream). Objetivo: demostrar pipeline **end-to-end**, ingestão incremental, qualidade de dados, SCD, otimização e produção de tabelas analíticas prontas para BI/ML.

## Estrutura
- `data_generator/` — simulador de eventos (gera arquivos JSON)
- `notebooks/` — notebooks PySpark: ingestão Bronze, processamento Silver, agregações Gold
- `sql/` — comandos Delta (MERGE, OPTIMIZE, VACUUM)
- `docs/` — diagramas e especificações

## Como rodar localmente (pré-requisito)
1. Python 3.9+  
2. Instalar libs: `pip install pandas faker tqdm`  
3. Rodar generator: `python data_generator/clickstream_simulator.py --out ./bronze/events --count 1000 --batch 10`

## Como rodar no Databricks (Community)
1. Faça upload dos notebooks para Databricks Workspace.  
2. Suba os arquivos gerados para `dbfs:/tmp/bronze/events/` (ou gere direto em `/dbfs/tmp/...`).  
3. Abra `01_bronze_ingestion.py` e rode para criar Delta Bronze.  
4. Rode `02_silver_processing.py` → Silver Delta (MERGE/SCD).  
5. Rode `03_gold_analytics.py` → Gold tables e dashboards.

## Dados usados
- Olist (Kaggle) → tabelas de pedidos, itens, clientes, pagamentos.
- Clickstream → eventos simulados (page_view, add_to_cart, purchase, session_id, user_id, timestamp).

## Principais features técnicas demonstradas
- Ingestão incremental (simulação de streaming por arquivos)  
- Delta Lake: ACID, Time Travel, MERGE (SCD), Optimize + ZORDER  
- Particionamento e layout de arquivos  
- Feature engineering para ML (LTV, RFM, funil, cohort)  
- Documentação e diagrama arquitetural

## Próximos passos
- Integrar Auto Loader / Structured Streaming (se disponível)  
- Exportar tabelas Gold para Power BI / Databricks SQL  
- Criar testes de data quality (great_expectations)


