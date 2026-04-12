# 📦 Previsão de Atraso em Entregas — E-commerce Brasileiro (Olist)

> Análise de Dados e Boas Práticas · Pós-Graduação  
> Dataset: [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

---

## 🎯 Visão Geral do Projeto

Este projeto tem como objetivo desenvolver uma análise exploratória completa de dados de e-commerce brasileiro, com foco na construção das bases para um modelo de **classificação binária** capaz de prever se um pedido será entregue **dentro ou fora do prazo estimado**.

A motivação parte de uma premissa real do setor de varejo: entregas fora do prazo impactam diretamente a experiência do cliente e geram custos operacionais relevantes.

---

## 🗂️ Estrutura do Projeto

```
├── MVP_Análise_de_dados_e_boas_práticas.ipynb       # Notebook principal com toda a análise
├── README.md                    # Este arquivo
└── Data/
    ├── olist_orders_dataset.csv
    ├── olist_order_items_dataset.csv
    ├── olist_customers_dataset.csv
    ├── olist_sellers_dataset.csv
    ├── olist_products_dataset.csv
    └── olist_geolocation_dataset.csv
```

---

## 📋 Definição do Problema

| Campo | Detalhe |
|---|---|
| **Tipo de Problema** | Aprendizado Supervisionado — Classificação Binária |
| **Variável Alvo** | `atrasado`: `1` = Entrega após a data estimada · `0` = Entrega no prazo |
| **Filtro Aplicado** | Apenas pedidos com status `delivered` (data real de entrega disponível) |

### Hipóteses Iniciais

1. A **distância entre vendedor e comprador** (venda interestadual) influencia diretamente o atraso
2. O **peso e as dimensões do produto** afetam o tempo de processamento logístico
3. Pedidos realizados em **datas de alta demanda** (ex: Black Friday) têm maior probabilidade de atraso

---

## 🗃️ Fonte dos Dados

Os dados são provenientes da **Olist**, plataforma de marketplace brasileira, e contêm aproximadamente **100 mil pedidos** realizados entre **2016 e 2018** em diversos marketplaces nacionais.

O dataset é relacional, composto por **8 tabelas** conectadas por chaves únicas. Para este projeto, **6 tabelas foram consolidadas** em um único dataset mestre via joins:

| Tabela | Conteúdo |
|---|---|
| `olist_orders_dataset` | Pedidos e suas datas (compra, envio, entrega estimada e real) |
| `olist_order_items_dataset` | Itens por pedido (produto, preço, frete) |
| `olist_customers_dataset` | Localização e identificação do comprador |
| `olist_sellers_dataset` | Localização e identificação do vendedor |
| `olist_products_dataset` | Características físicas e categoria do produto |
| `olist_geolocation_dataset` | Coordenadas geográficas por CEP |

---

## 🔑 Dicionário de Dados (Principais Atributos)

| Atributo | Tipo | Descrição |
|---|---|---|
| `order_id` | `object` | Identificador único do pedido |
| `customer_id` | `object` | Chave de ligação com a tabela de clientes |
| `order_status` | `object` | Status do pedido (filtrado para `delivered`) |
| `order_purchase_timestamp` | `datetime64` | Data e hora da compra |
| `order_delivered_customer_date` | `datetime64` | Data real de recebimento pelo cliente |
| `order_estimated_delivery_date` | `datetime64` | Prazo estimado informado no momento da compra |
| `price` | `float64` | Preço de venda do produto (R$) |
| `freight_value` | `float64` | Valor do frete pago pelo cliente (R$) |
| `product_weight_g` | `float64` | Peso do produto em gramas |
| `customer_state` | `object` | Estado (UF) do comprador |
| `seller_state` | `object` | Estado (UF) do vendedor |
| `atrasado` *(target)* | `int64` | `1` = atrasado · `0` = no prazo |

---

## 📊 Etapas da Análise

### 1. Carregamento e Integração dos Dados
- Leitura das 6 tabelas via URL (GitHub)
- Consolidação via `inner joins` em um único dataset mestre
- Conversão de colunas de data para `datetime64`

### 2. Análise Descritiva
- Verificação de shape, tipos de dados e primeiras linhas
- Identificação de valores nulos, outliers e inconsistências
- Resumo estatístico: mínimo, máximo, mediana, moda, média, desvio-padrão e valores ausentes

### 3. Visualizações Exploratórias
- Distribuição da variável alvo (No Prazo vs. Atrasado)
- Taxa de atraso por estado do cliente (Top 10)
- Distribuição de preço e frete
- Boxplot de frete vs. status de atraso
- Identificação de outliers de peso
- Mapa de correlação entre variáveis numéricas

### 4. Análises Avançadas *(além do checklist)*
- **Análise Temporal**: proporção de atrasos e volume de pedidos por mês
- **Análise por Categoria**: volume de pedidos vs. taxa de atraso nas top 10 categorias

### 5. Pré-processamento
- Remoção de registros sem data de entrega real
- Imputação de valores nulos de peso pela mediana
- Criação da feature `venda_externa` (venda interestadual: 0 ou 1)
- Transformação logarítmica de `freight_value` e `product_weight_g`
- One-Hot Encoding de `customer_state`

### 6. Seleção de Variáveis
- Prevenção de *data leakage*: exclusão de datas reais do conjunto de features
- Dataset final com **29 atributos** para uso em modelagem futura

---

## 📈 Principais Insights

- **~8% dos pedidos** foram entregues com atraso no período analisado
- O **valor do frete** e o **peso do produto** apresentam alta assimetria e foram tratados com transformação logarítmica (redução de skewness de 5.64 → -0.02 no frete)
- **Atrasos variam por região**: estados mais distantes dos centros logísticos apresentam maior proporção de atraso
- **Atrasos variam por categoria**: categorias como *beleza e saúde* e *cama, mesa e banho* combinam alto volume com taxas de atraso acima da média
- A correlação linear entre as variáveis numéricas e o target é baixa, sugerindo que o atraso é um **fenômeno multifatorial** e não linear

---

## 🛠️ Tecnologias Utilizadas

```
Python 
├── pandas          → Manipulação e integração dos dados
├── numpy           → Operações numéricas e transformações
├── matplotlib      → Visualizações base
├── seaborn         → Visualizações estatísticas
└── scikit-learn    → Estrutura para modelagem futura
```
---

## 👤 Autor

**Julio Losa**  
Pós-Graduação Latu Sensu, Ciência de dados e Analytics

Sprint: Análise de dados e Boas práticas

---

> *"O atraso logístico não é determinado por uma única variável, mas pela interação entre fatores geográficos, físicos e temporais."*
