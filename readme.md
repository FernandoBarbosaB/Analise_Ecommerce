# Análise de Dados - Ecommerce (melhorar o nome)

Este documento demonstra o desenvolvimento técnico do projeto de análise de dados sobre a base de dados de uma empresa de E-commerce fictícia, utilizando ferramentas como Databricks e StarUML.

## 🏭 Arquitetura

"colocar imagem da arquitetura"

## ⚙️ Extração dos Dados

Este processo aborda a extração dos dados do site UI Bakery, que disponibiliza um banco de dados fictício de um E-commerce. Os dados são obtidos utilizando as credenciais fornecidas pela [UI Bakery](https://uibakery.io/sql-playground).




### 🔩 Layout dos Dados

(colocar a imagem do diagrma)



 - Employee
 

|     **variável**     |  **tipo**   |              **descrição**               |
|:-------------------:|:----------:|:---------------------------------------:|
|  employee_number    |  INTEGER   | Número do funcionário                   |
|  last_name          |  STRING    | Sobrenome do funcionário                |
|  first_name         |  STRING    | Primeiro nome do funcionário            |
|  extension          |  STRING    | Número da extensão telefônica           |
|  email              |  STRING    | Endereço de e-mail do funcionário       |
|  office_code        |  STRING    | Código do escritório onde trabalha      |
|  reports_to         |  INTEGER   | Número do funcionário supervisor        |
|  job_Title          |  STRING    | Cargo/posição do funcionário            |


- Customers

|     **variável**         |   **tipo**   |                    **descrição**                  |
|:------------------------:|:------------:|:-----------------------------------------------:|
|   customer_number        |   INTEGER    | Número do cliente                               |
|   customer_name          |   STRING     | Nome do cliente                                 |
|   contact_last_name      |   STRING     | Sobrenome do contato do cliente                 |
|   contact_first_name     |   STRING     | Primeiro nome do contato do cliente             |
|   phone                  |   STRING     | Número de telefone do cliente                   |
|   address_line1          |   STRING     | Linha de endereço 1 do cliente                  |
|   address_line2          |   STRING     | Linha de endereço 2 do cliente (opcional)       |
|   city                   |   STRING     | Cidade do cliente                                |
|   state                  |   STRING     | Estado do cliente (ou região/administração)      |
|   postal_code            |   STRING     | Código postal do cliente                         |
|   country                |   STRING     | País do cliente                                  |
|   sales_rep_employee_number | INTEGER    | Número do funcionário responsável pelas vendas   |
|   credit_limit           |   DECIMAL(10,2)  | Limite de crédito do cliente (formato decimal)   |

- Orders

|    **variável**    |   **tipo**  |                **descrição**               |
|:------------------:|:-----------:|:----------------------------------------:|
|   order_number     |   INTEGER   | Número do pedido                         |
|   order_date       |    DATE     | Data do pedido                           |
|   required_date    |    DATE     | Data requerida para entrega              |
|   shipped_date     |    DATE     | Data de envio do pedido                  |
|   status           |   STRING    | Status do pedido                         |
|   comments         |   STRING    | Comentários adicionais sobre o pedido    |
|   customer_number  |   INTEGER   | Número do cliente associado ao pedido    |

- Order Details

|    **variável**     |      **tipo**      |                    **descrição**                   |
|:-------------------:|:------------------:|:-----------------------------------------------:|
|   order_number      |      INTEGER       | Número do pedido                                |
|   product_code      |      STRING        | Código do produto                               |
|   quantity_ordered  |      INTEGER       | Quantidade do produto pedida                    |
|   price_each        |   DECIMAL(10,2)    | Preço unitário do produto (formato decimal)    |
|   order_line_number |       SHORT        | Número da linha do pedido                       |

- Offices

|   **variável**    |   **tipo**   |                **descrição**                |
|:-----------------:|:-----------:|:-----------------------------------------:|
|   office_code     |   STRING    | Código do escritório                      |
|   city            |   STRING    | Cidade onde o escritório está localizado   |
|   phone           |   STRING    | Número de telefone do escritório          |
|   address_line1   |   STRING    | Linha de endereço 1 do escritório         |
|   address_line2   |   STRING    | Linha de endereço 2 do escritório (opcional) |
|   state           |   STRING    | Estado onde o escritório está localizado  |
|   country         |   STRING    | País onde o escritório está localizado    |
|   postal_code     |   STRING    | Código postal do escritório               |
|   territory       |   STRING    | Território atribuído ao escritório        |

- Payments

|   **variável**     |   **tipo**  |                **descrição**              |
|:------------------:|:-----------:|:---------------------------------------:|
|   customer_number  |   INTEGER   | Número do cliente                       |
|   check_number     |   STRING    | Número do cheque                        |
|   payment_date     |    DATE     | Data do pagamento                       |
|   amount           | DECIMAL(10,2)| Valor do pagamento (formato decimal)    |

- Products

|   **variável**         |   **tipo**     |                   **descrição**                  |
|:----------------------:|:--------------:|:--------------------------------------------:|
|   product_code         |   STRING       | Código do produto                            |
|   product_name         |   STRING       | Nome do produto                              |
|   product_line         |   STRING       | Linha de produtos à qual o produto pertence |
|   product_scale        |   STRING       | Escala do produto (tamanho/escala)           |
|   product_vendor       |   STRING       | Nome do fornecedor do produto                |
|   product_description  |   STRING       | Descrição detalhada do produto               |
|   quantity_in_stock    |   SHORT        | Quantidade disponível em estoque             |
|   buy_price            |   DECIMAL(10,2)| Preço de compra do produto (formato decimal)|
|   msrp                 |   DECIMAL(10,2)| Preço sugerido pelo fabricante (formato decimal)|

- Product Lines

|    **variável**      |   **tipo**  |             **descrição**                  |
|:--------------------:|:-----------:|:----------------------------------------:|
|   product_line       |   STRING    | Linha de produtos                         |
|   text_description   |   STRING    | Descrição do produto em texto             |
|   html_description   |   STRING    | Descrição do produto em formato HTML      |
|   image              |   BINARY    | Imagem relacionada à linha de produtos    |


## 📦 Desenvolvimento


O projeto foi desenvolvido em três etapas principais:
<b>
1. Extração
2. Tratamento
3.  Análise
</b>

Para cada etapa, um notebook foi criado utilizando a ferramenta Databricks.
Seguindo as boas práticas, a arquitetura do projeto foi estruturada da seguinte forma:

- Camada Bronze: Dados brutos
- Camada Silver: Dados tratados
- Camada Gold: Análise dos dados

A seguir, apresentaremos o processo de desenvolvimento em cada etapa.

<h3> Extração </h3>

Inicialmente, foi criada a sessão do Spark (SparkSession) com o nome "Extração de Dados Database Type Ecommerce UI Bakery". Em seguida, desenvolvemos uma função chamada 'ler_dados' com o objetivo de ler os dados de uma tabela específica em um banco de dados PostgreSQL usando o PySpark. Utilizamos as credenciais e a string de conexão fornecidas pela UI Bakery e a biblioteca psycopg2 para obter informações desse banco de dados.

Após verificar as informações disponíveis, selecionamos as tabelas que serão usadas para a realização deste projeto:

- customers
- employees
- offices
- orderdetails
- orders
- payments
- product_lines
- products

Com as funções definidas e os respectivos nomes das tabelas selecionados, foi realizado o processo de Extração de dados. Além disso, o shape do dataframe e o seu schema foram visualizados.

Após esse processo, os dados brutos foram armazenados na camada 'bronze' utilizando o formato delta.

<h3> Tratamento </h3>
Nesta seção, será apresentado o processo de tratamento de cada dataframe.


Inicialmente, foi criada a sessão do Spark (SparkSession) com o nome "Tratamento de Dados Database Type Ecommerce UI Bakery".
Foram desenvolvidas duas funções, sendo:

- exibir_info_df - para exibir informações do dataframe, como o shape e também o seu esquema.
- verificar_dados_nulos - uma função que faz a contagem de valores nulos por coluna.

Neste notebook, os dados brutos foram lidos da camada bronze, e as funções foram executadas para obtermos informações de cada dataframe. Em seguida, os dados foram tratados de acordo com as regras de negócio definidas.
Durante o tratamento, verificou-se a tipagem dos dados e também a presença de valores ausentes e nulos.
Alguns dataframes apresentavam colunas com mais de 80% de valores nulos, enquanto outros continham números junto aos nomes, mostrando a necessidade do tratamento.


Para esse processo, utilizaram-se técnicas de tratamento de valores nulos e ausentes com o objetivo de não afetar nenhuma análise posterior e garantir que os dataframes não apresentassem dados nulos.

Após o tratamento dos dados brutos, os dados tratados foram salvos na camada 'silver'.


<h3> Análise </h3>


Nesta seção, serão apresentadas as respostas das perguntas definidas no projeto.<br>
Para cada pergunta, serão fornecidas duas soluções: uma utilizando SQL e outra com PySpark.<br>
Inicialmente, foi criada a sessão do Spark (SparkSession) com o nome "Análise de Dados Database Type Ecommerce UI Bakery".<br>
Neste notebook, reutilizamos a função 'exibir_info_df' apresentada na etapa de tratamento.


> <b>Qual país possui a maior quantidade de itens cancelados?</b>

>  O país com maior quantidade de itens cancelados é a Espanha, com 605 itens cancelados

(imagem gráfico resp q1)

Dataframes abordados para essa questão:

- Orders
- Order Details
- Customers


> <b>Qual o faturamento da linha de produto mais vendido, considere os itens com status 'Shipped', cujo o pedido foi realizado no ano de 2005</b>

> O faturamento da linha de produto mais vendido no ano de 2005 e com o status Shipped é: <b> 
Classic Cars -  $ 15.559,72</b>

(imagem graf resp q2)

Dataframes abordados para essa questão:

- Orders
- Orders Details
- Products



> <b>Nome, sobrenome e email dos vendedores do Japão, o local-part do e-mail deve estar mascarado.</b>

|   **first_name** | **last_name**|              **email**                   |
|:----------------:|:------------:|:----------------------------------------:|
|   Mami           |   Nishi      |      *****@classicmodelcars.com          |
|   Yoshimi        |   Kato       |      *****@classicmodelcars.com          |







## 🚧 Descrição dos arquivos

- join_extract.ipynb = Notebook responsável por extrair os dados e armazenar para a camada bronze

- join_tratamento.ipynb = Notebook responsável pelo tratamento dos dados

- join_analise.ipynb = Notebook responsável pelas análise e respostas do projeto

## 🛠️ Construído com



* [Databricks](https://www.databricks.com/) - Databricks Community - Ferramenta de Desenvolvimento para trabalhar com Spark

* [UI Bakery](https://uibakery.io/sql-playground) - Ferramenta para Desenvolvimento, responsável por disponibilizar o Banco de Dados PostgresSQL

* [StarUML](https://staruml.io/) - Ferramenta para Desenvolvimento de diagramas de Entidade e Relacionamento


## 🏃 Autor


* **Fernando Barbosa**  - [github](https://github.com/FernandoBarbosaB)
