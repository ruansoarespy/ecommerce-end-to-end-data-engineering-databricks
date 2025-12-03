# 🛒 E-commerce Data Engineering — Databricks Medallion Architecture  
## Projeto End-to-End usando Olist Dataset (PySpark + Delta Lake)

Este repositório apresenta um pipeline completo de Engenharia de Dados utilizando a Arquitetura **Medallion (Bronze → Silver → Gold)** no **Databricks**, processando o dataset público **Olist**.  
O projeto replica o fluxo real de um ambiente corporativo de dados: ingestão, processamento, qualidade, modelagem e disponibilização analítica.

---

## 📂 Estrutura Geral do Projeto
.
├── bronze/              # Dados crus do Olist em Delta
├── silver/              # Dados tratados e integrados
├── gold/                # Tabelas analíticas e métricas
├── notebooks/           # Notebooks organizados por camada
├── sql/                 # Comandos Delta (OPTIMIZE, VACUUM, MERGE)
├── docs/                # Diagramas, dicionário, documentação
└── README.md            # Este arquivo


/data
/bronze # CSVs brutos
/silver # tabelas Delta limpas
/gold # modelos analíticos

/docs
schema_reference.md
data_quality.md

README.md

yaml
Copiar código

---

## 🧱 Arquitetura Medallion

### 🥉 Bronze — Raw Layer
- Recebe arquivos **CSV** diretamente do dataset Olist.  
- Nenhuma transformação aplicada.  
- Apenas ingestão com `inferSchema` e `header=True`.  
- Armazenado como **Delta Lake** para permitir time travel e versionamento.

**Objetivo:** garantir que os dados brutos sejam preservados sem alteração.

---

### 🥈 Silver — Refined Layer
- Padronização de colunas  
- Conversão de tipos  
- Normalização de datas  
- Remoção de duplicatas  
- Correção de registros inconsistentes  
- Enriquecimento leve (ex: join cliente-endereço)

**Camada já concluída no projeto.**

---

### 🥇 Gold — Business Layer  
Camada orientada ao negócio, pronta para BI e análises sofisticadas.

Tabelas principais:

#### **1. order_facts**
- Fato de pedidos com granularidade por pedido-item  
- Valores de receita, frete, totais e prazos

#### **2. customer_dim**
- Dimensão de clientes com histórico agregável

#### **3. product_dim**
- Dimensão de produtos, categorias traduzidas e medidas

#### **4. seller_dim**
- Dimensão de vendedores

#### **5. rfm_customer_features**
- Recency  
- Frequency  
- Monetary  
Prontos para clustering ou LTV models.

---

## 🚀 Tecnologias
- **Databricks Community Edition**
- **PySpark**
- **Delta Lake**
- **ETL/ELT**
- **Business Analytics**
- **Power BI**

---

## 📌 Fluxo do Pipeline

1. Upload dos CSVs no DBFS (`/ecommerce/bronze/2/…`)
2. Leitura e armazenamento em Delta (Bronze)
3. Limpeza, conversões e dedupe (Silver)
4. Modelagem dimensional e tabelas fato/dimensão (Gold)
5. Consumo no Power BI (via Databricks SQL Endpoint)

---

## 🎯 Objetivos do Projeto

- Demonstrar domínio real em **Data Engineering**
- Criar um pipeline completo e reproduzível
- Aplicar boas práticas (SCD, dedupe, padronização)
- Criar modelos analíticos sólidos para BI
- Mostrar senioridade em Databricks + Delta Lake

---

## 📊 Dashboard (Power BI)
O dashboard final inclui:

- Vendas por categoria  
- Faturamento por mês  
- Análise de entregas (SLA, atrasos, lead time)  
- Mapa de geolocalização  
- Customer RFM  

---

## 📘 Dataset
O projeto utiliza exclusivamente o dataset **Olist Brazilian E-Commerce**, composto por:

- Pedidos  
- Clientes  
- Itens  
- Pagamentos  
- Produtos  
- Vendedores  
- Categorias traduzidas  
- Geolocalização

---

## 📎 Próximas Etapas
- Implementar testes de qualidade (Great Expectations)  
- Adicionar monitoramento (Delta Live Tables)  
- Adicionar particionamento e ZORDER  
- Criar pipeline Airflow opcional  

---

Contato / Autor

**Ruan Ferreira Soares** — https://www.linkedin.com/in/ruan-soares123/
Engenharia de Dados • PySpark • Delta Lake • Databricks  

