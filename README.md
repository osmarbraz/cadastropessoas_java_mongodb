# Cadastro de Pessoas – Java + MongoDB

## 1. Descrição do Projeto

Este projeto demonstra a integração entre a linguagem Java e o banco de dados NoSQL MongoDB, utilizando o Maven para gerenciamento de dependências.

A aplicação implementa um sistema simples de cadastro de pessoas com seu nome, permitindo realizar operações de inclusão, consulta, atualização e exclusão de documentos no MongoDB.

O objetivo é demonstrar na prática as operações CRUD (*Create, Read, Update e Delete*) utilizando o MongoDB Java Sync Driver.

## 2. Tecnologias Utilizadas

- **Java:** linguagem de programação utilizada no desenvolvimento.
- **Maven:** ferramenta para gerenciamento de dependências e construção do projeto.
- **MongoDB:** banco de dados NoSQL orientado a documentos.
- **MongoDB Java Sync Driver:** biblioteca utilizada para comunicação entre Java e MongoDB.

## 3. Pré-requisitos

Antes de executar o projeto, é necessário possuir:

- JDK instalado (recomendado Java 8 ou superior).
- Apache Maven instalado.
- MongoDB Server instalado e em execução.
- MongoDB configurado para aceitar conexões em `localhost:27017`.

## 4. Dependência Maven

O projeto utiliza o driver oficial do MongoDB para Java.

Adicione a seguinte dependência ao arquivo `pom.xml`:

```xml
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongodb-driver-sync</artifactId>
    <version>5.5.1</version>
</dependency>
```

## 5. Estrutura do Projeto

A organização recomendada para o projeto Maven é:

```text
cadastropessoas_java_mongodb/
│
├── pom.xml
├── .gitignore
├── README.md
├── LICENSE
└── src/
    └── main/
        └── java/
            └── Principal.java
```

## 6. Configuração da Conexão

A aplicação estabelece conexão com o MongoDB utilizando a seguinte URI:

```java
String uri = "mongodb://localhost:27017";
```

O banco de dados e a coleção utilizados são:

```java
MongoDatabase banco = clienteMongoDB.getDatabase("test");

MongoCollection<Document> dbTest = banco.getCollection("test");
```

- Banco de dados: `test`
- Coleção: `test`

Caso o banco de dados ou a coleção ainda não existam, o MongoDB os criará quando ocorrer a primeira operação de gravação.

**Observação:** certifique-se de que o serviço do MongoDB esteja em execução antes de iniciar a aplicação.

## 7. Funcionalidades do Sistema

A aplicação apresenta um menu interativo no terminal com as seguintes operações:

| Opção | Funcionalidade | Operação MongoDB |
|---|---|---|
| 1 | Cadastrar pessoa | `insertOne()` |
| 2 | Listar pessoas | `find()` |
| 3 | Consultar pessoa por ID | `find()` |
| 4 | Atualizar pessoa | `updateOne()` |
| 5 | Excluir pessoa | `deleteOne()` |
| 9 | Encerrar programa | — |

### 7.1. Cadastrar Pessoa

Permite inserir um novo documento na coleção `test`.

```java
Document pessoadoc = new Document().append("pessoa", pessoa);

InsertOneResult resultado = dbTest.insertOne(pessoadoc);
```

O MongoDB gera automaticamente o identificador `_id` do documento.

### 7.2. Listar Pessoas

Consulta os documentos cadastrados e apresenta os resultados em ordem crescente pelo campo `nome`.

```java
for (Document pessoadoc:dbTest.find().sort(ascending("nome"))) {
    exibirDbTest(pessoadoc);
}
```

### 7.3. Consultar pessoa por ID

Permite localizar um documento utilizando seu identificador `_id`.

```java
Document mensagem = dbTest.find(eq("_id", id)).first();
```

O programa valida o formato do identificador utilizando `ObjectId.isValid()`.

### 7.4. Atualizar Pessoa

Permite modificar o campo `nome` de pessoa de um documento existente.

```java
dbTest.updateOne(eq("_id", id), combine(set("nome", nome)));
```

A atualização utiliza o operador `$set`, que modifica somente o campo informado.

### 7.5. Excluir Pessoa

Permite excluir um documento pelo identificador `_id`, solicitando confirmação do usuário antes da operação.

```java
DeleteResult resultado = dbTest.deleteOne(eq("_id", id));
```

## 8. Executando o Projeto

**Passo 1 – Iniciar o MongoDB**

Verifique se o servidor MongoDB está em execução e disponível na porta `27017`.

**Passo 2 – Abrir o projeto**

Abra o projeto em uma IDE de sua preferência, como IntelliJ IDEA, Eclipse ou Visual Studio Code.

**Passo 3 – Compilar o projeto**

No terminal, dentro do diretório do projeto, execute:

```bash
mvn clean compile
```

**Passo 4 – Executar o programa**

Caso o projeto esteja configurado com o plugin Maven Exec, execute:

```bash
mvn exec:java -Dexec.mainClass="Principal"
```

## 9. Exemplo de Utilização

Ao executar o programa, será apresentado o seguinte menu:

```text
=== MENU CADASTRO DE PESSOAS ===
1 - Cadastrar pessoa
2 - Listar pesssoas
3 - Consultar pessoa por ID
4 - Atualizar pessoa
5 - Excluir pessoa
9 - Sair

Escolha uma opcao:
```

Após cadastrar uma pessoa, o MongoDB armazenará um documento semelhante a:

```json
{
    "_id": "ObjectId(...)",
    "nome": "Joao"
}
```

O campo `_id` é criado automaticamente pelo MongoDB e utilizado para identificar cada documento.

## 10. Conceitos Trabalhados

Com este projeto, são explorados os seguintes conceitos:

- Integração entre Java e MongoDB.
- Gerenciamento de dependências com Maven.
- Banco de dados NoSQL orientado a documentos.
- Estrutura de documentos BSON.
- Identificadores `ObjectId`.
- Operações CRUD.
- Inserção e consulta de documentos.
- Ordenação de resultados.
- Atualização de documentos com `$set`.
- Exclusão de documentos.
- Tratamento de exceções com `MongoException`.

## 11. Considerações Finais

O projeto apresenta uma aplicação prática de integração entre Java e MongoDB, permitindo compreender como uma aplicação pode realizar operações de persistência de dados em um banco de dados orientado a documentos.

A implementação demonstra as principais operações de manipulação de dados e serve como ponto de partida para o desenvolvimento de aplicações mais completas utilizando Java, Maven e MongoDB.

Compare a diferença de se trabalhar com um banco de dados relacional no projeto:https://github.com/osmarbraz/cadastropessoas_java_mysql/