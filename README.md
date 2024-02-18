# Projeto: Curso Udemy - Arquitetura de Microsserviços: Padrão Saga Orquestrado

### Curso Udemy
Para acessar o curso na plataforma, basta acessar esta URL: https://www.udemy.com/course/arquitetura-de-microsservicos-padrao-saga-orquestrado/

### Repositório oficial

Para acessa o repositório oficial basta acessar a URL: https://github.com/vhnegrisoli/curso-udemy-microsservicos-padrao-saga-coreografado

### Sumário:

* [Tecnologias](#tecnologias)
* [Ferramentas utilizadas](#ferramentas-utilizadas)
* [Arquitetura Proposta](#arquitetura-proposta)
* [Execução do projeto](#execu%C3%A7%C3%A3o-do-projeto)
  * [01 - Execução geral via docker-compose](#01---execu%C3%A7%C3%A3o-geral-via-docker-compose)
  * [02 - Execução geral via automação com script em Python](#02---execu%C3%A7%C3%A3o-geral-via-automa%C3%A7%C3%A3o-com-script-em-python)
  * [03 - Executando os serviços de bancos de dados e Message Broker](#03---executando-os-servi%C3%A7os-de-bancos-de-dados-e-message-broker)
  * [04 - Executando manualmente via CLI](#04---executando-manualmente-via-cli)
* [Acesso ao MongoDB](#acesso-ao-mongodb)  

## Tecnologias

[Voltar ao início](#sum%C3%A1rio)

* **Java 17**
* **Spring Boot 3**
* **Apache Kafka**
* **API REST**
* **PostgreSQL**
* **MongoDB**
* **Docker**
* **docker-compose**
* **Redpanda Console**

# Ferramentas utilizadas

[Voltar ao início](#sum%C3%A1rio)

* **IntelliJ IDEA Community Edition**
* **Docker**
* **Gradle**


## Arquitetura Proposta

[Voltar ao início](#sum%C3%A1rio)

No curso, desenvolveremos a seguinte aquitetura:

![Arquitetura](Conte%C3%BAdos/Arquitetura%20Proposta.png)

Em nossa arquitetura, teremos 4 serviços:

* **Order-Service**: microsserviço responsável apenas por gerar um pedido inicial, e receber uma notificação. Aqui que teremos endpoints REST para inciar o processo e recuperar os dados dos eventos. O banco de dados utilizado será o MongoDB.
* **Product-Validation-Service**: microsserviço responsável por validar se o produto informado no pedido existe e está válido. Este microsserviço guardará a validação de um produto para o ID de um pedido. O banco de dados utilizado será o PostgreSQL.
* **Payment-Service**: microsserviço responsável por realizar um pagamento com base nos valores unitários e quantidades informadas no pedido. Este microsserviço guardará a informação de pagamento de um pedido. O banco de dados utilizado será o PostgreSQL.
* **Inventory-Service**: microsserviço responsável por realizar a baixa do estoque dos produtos de um pedido. Este microsserviço guardará a informação da baixa de um produto para o ID de um pedido. O banco de dados utilizado será o PostgreSQL.

Todos os serviços da arquitetura irão subir através do arquivo **docker-compose.yml**.

## Execução do projeto

[Voltar ao início](#sum%C3%A1rio)

Há várias maneiras de executar os projetos:

1. Executando tudo via `docker-compose`
2. Executando tudo via `script` de automação que eu disponibilizei (`build.py`)
3. Executando apenas os serviços de bancos de dados e message broker (Kafka) separadamente
4. Executando as aplicações manualmente via CLI (`java -jar` ou `gradle bootRun` ou via IntelliJ)

Para rodar as aplicações, será necessário ter instalado:

* **Docker**
* **Java 17**
* **Gradle 7.6 ou superior**

### 01 - Execução geral via docker-compose

[Voltar ao nível anterior](#execu%C3%A7%C3%A3o-do-projeto)

Basta executar o comando no diretório raiz do repositório:

`docker-compose up --build -d`

**Obs.: para rodar tudo desta maneira, é necessário realizar o build das 5 aplicações, veja nos passos abaixo sobre como fazer isto.**

### 02 - Execução geral via automação com script em Python

[Voltar ao nível anterior](#execu%C3%A7%C3%A3o-do-projeto)

Basta executar o arquivo `build.py`. Para isto, **é necessário ter o Python 3 instalado**.

Para executar, basta apenas executar o seguinte comando no diretório raiz do repositório:

`python build.py`

Será realizado o `build` de todas as aplicações, removidos todos os containers e em sequência, será rodado o `docker-compose`.

### 03 - Executando os serviços de bancos de dados e Message Broker

[Voltar ao nível anterior](#execu%C3%A7%C3%A3o-do-projeto)

Para que seja possível executar os serviços de bancos de dados e Message Broker, como MongoDB, PostgreSQL e Apache Kafka, basta ir no diretório raiz do repositório, onde encontra-se o arquivo `docker-compose.yml` e executar o comando:

`docker-compose up --build -d order-db kafka product-db payment-db inventory-db`

Como queremos rodar apenas os serviços de bancos de dados e Message Broker, é necessário informá-los no comando do `docker-compose`, caso contrário, as aplicações irão subir também.

Para parar todos os containers, basta rodar:

`docker-compose down` 

Ou então:

`docker stop ($docker ps -aq)`
`docker container prune -f`

### 04 - Executando manualmente via CLI

[Voltar ao nível anterior](#execu%C3%A7%C3%A3o-do-projeto)

Antes da execução do projeto, realize o `build` da aplicação indo no diretório raiz e executando o comando:

`gradle build -x test`

Para executar os projetos com Gradle, basta entrar no diretório raiz de cada projeto, e executar o comando:

`gradle bootRun` 

Ou então, entrar no diretório: `build/libs` e executar o comando:

`java -jar nome_do_jar.jar`


### Acesso ao MongoDB

[Voltar ao início](#sum%C3%A1rio)

Para conectar-se ao MongoDB via linha de comando (cli) diretamente do docker-compose, basta executar o comando abaixo:

**docker exec -it order-db mongosh "mongodb://admin:123456@localhost:27017"**

Para listar os bancos de dados existentes:

**show dbs**

Para selecionar um banco de dados:

**use admin**

Para visualizar as collections do banco:

**show collections**

Para realizar queries e validar se os dados existem:

**db.order.find()**

**db.event.find()**

**db.order.find(id=ObjectId("65006786d715e21bd38d1634"))**

**db.order.find({ "products.product.code": "COMIC_BOOKS"})**

