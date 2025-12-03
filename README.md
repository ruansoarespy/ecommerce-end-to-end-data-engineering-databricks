# 📘 Projeto Lakehouse E-commerce – Databricks (Bronze → Silver → Gold)

Este projeto implementa um pipeline completo de Engenharia de Dados utilizando o padrão **Medallion Architecture** (Bronze / Silver / Gold) no **Databricks Lakehouse**.  
O objetivo é transformar dados brutos do e-commerce Olist em:

- tabelas otimizadas e limpas (Silver)  
- um modelo dimensional (Gold)  
- visões analíticas e KPIs  
- dashboards e análises avançadas  

---

## 🔥 Visão Geral da Arquitetura

```
notebooks/
│
├── bronze/
│   └── bronze_ingestion.ipynb
│
├── silver/
│   └── silver_processing.ipynb
│
├── gold/
│   └── gold_processing.ipynb
│
└── gold_modeling/
    ├── dim_customers.ipynb
    ├── dim_products.ipynb
    ├── dim_sellers.ipynb
    ├── dim_dates.ipynb
    ├── fact_orders.ipynb
    ├── fact_order_items.ipynb
    ├── fact_payments.ipynb
    ├── fact_reviews.ipynb
    └── kpi_views.ipynb
```

Os dados são armazenados dentro do Databricks, no Volume Lakehouse:

```
/Volumes/ecommerce_cat/ecommerce_schema/bronze
/Volumes/ecommerce_cat/ecommerce_schema/silver
/Volumes/ecommerce_cat/ecommerce_schema/gold
```

A pasta `/Volumes` **não faz parte do GitHub**, apenas o código é versionado.

---

# 🟫 1) Camada Bronze – Ingestão Bruta

A camada Bronze recebe os arquivos originais do dataset Olist:

- customers.csv  
- orders.csv  
- order_items.csv  
- products.csv  
- sellers.csv  
- order_payments.csv  
- order_reviews.csv  
- geolocation.csv  
- product_category_translation.csv  

Funções da Bronze:

- leitura de CSVs  
- aplicação dos schemas  
- padronização inicial  
- gravação em **Delta Lake**  
- armazenamento seguro no Volume Bronze  

---

# 🥈 2) Camada Silver – Tratamento e Normalização

A Silver limpa e organiza os dados:

- remoção de duplicatas  
- correção de tipos  
- padronização de colunas  
- tratamento de valores faltantes  
- enriquecimentos simples  
- criação de tabelas analíticas estáveis  

Todas armazenadas em:

```
/Volumes/ecommerce_cat/ecommerce_schema/silver/<tabela>
```

Exemplos de tabelas Silver:

- customers  
- orders  
- products  
- sellers  
- order_items  
- order_payments  
- order_reviews  
- geolocation  

---

# 🥇 3) Camada Gold – Modelo Dimensional

A camada Gold contém o Data Warehouse final com modelo estrela:

## 📁 Dimensões criadas

- **dim_customers**
- **dim_products**
- **dim_sellers**
- **dim_dates**

### Exemplo (Dim Customers)

Selecionamos apenas as colunas importantes e removemos duplicatas:

```python
dim_customers = (
    df.select(
        "customer_id",
        "customer_unique_id",
        "customer_city",
        "customer_state",
        "customer_zip_code_prefix"
    )
    .dropDuplicates(["customer_id"])
)

dim_customers.write.format("delta").mode("overwrite").save(f"{gold}/dim_customers")

spark.sql("""
CREATE OR REPLACE VIEW ecommerce_cat.ecommerce_schema.dim_customers AS
SELECT * FROM delta.`/Volumes/ecommerce_cat/ecommerce_schema/gold/dim_customers`
""")
```

## 📁 Tabelas Fato

- **fact_orders**
- **fact_order_items**
- **fact_payments**
- **fact_reviews**

Cada fato contém chaves estrangeiras + métricas (medidas numéricas).

---

# 📊 4) Criação de KPIs e Visões Analíticas

No arquivo `kpi_views.ipynb` foram criadas views SQL que serão consumidas por ferramentas de BI.

### KPI – Receita total

```sql
CREATE OR REPLACE VIEW ecommerce_cat.ecommerce_schema.kpi_revenue AS
SELECT SUM(price + freight_value) AS revenue
FROM ecommerce_cat.ecommerce_schema.fact_order_items;
```

### KPI – Top 5 estados que mais compram

```sql
CREATE OR REPLACE VIEW ecommerce_cat.ecommerce_schema.kpi_top_states AS
SELECT c.customer_state, SUM(oi.price) AS revenue
FROM ecommerce_cat.ecommerce_schema.fact_order_items oi
JOIN ecommerce_cat.ecommerce_schema.fact_orders o USING(order_id)
JOIN ecommerce_cat.ecommerce_schema.dim_customers c USING(customer_id)
GROUP BY c.customer_state
ORDER BY revenue DESC
LIMIT 5;
```

### KPI – Média de Avaliações

```sql
CREATE OR REPLACE VIEW ecommerce_cat.ecommerce_schema.kpi_avg_review AS
SELECT AVG(review_score_clean) AS avg_review
FROM ecommerce_cat.ecommerce_schema.gold_reviews;
```

---

# 📈 5) Dashboards – O que é possível fazer

Com o modelo dimensional pronto, é possível construir dashboards de alto nível.

## 📌 Dashboard de Vendas

- receita total  
- receita por categoria  
- receita por estado  
- top 10 produtos  
- quantidade de pedidos por dia  
- ticket médio  
- mapa geográfico  

## 📌 Dashboard de Logística

- tempo médio de entrega  
- atraso por estado  
- atraso por vendedor  
- volume de pedidos por semana  

## 📌 Dashboard de Satisfação

- média geral de reviews  
- distribuição das notas  
- comentários positivos e negativos  
- indicadores por categoria  

---


Contato / Autor

**Ruan Ferreira Soares** — https://www.linkedin.com/in/ruan-soares123/
Engenharia de Dados • PySpark • Delta Lake • Databricks  

