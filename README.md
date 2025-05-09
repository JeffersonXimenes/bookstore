# Bookstore

Esse projeto foi desenvolvido utilizando Spring Boot, Redis e MySQL. É utilizado também inserção de dados e geração da documentação com open api

# Funcionalidades

A API expõe os seguintes endpoints:

### **1. Buscar todos os livros (com paginação)**
**GET /books**
#### **Parâmetros de consulta (query params)**
| Parâmetro    | Tipo   | Obrigatório | Padrão | Mínimo | Máximo | Descrição |
|-------------|--------|-------------|---------|--------|--------|------------|
| `page`      | int    | Não         | 0       | 0      | -      | Número da página (baseado em zero) |
| `size`      | int    | Não         | 10      | 1      | 50    | Quantidade de itens por página |

#### **Exemplo de requisição**
```http GET /books?page=0&size=5```

### **Exemplo de resposta (200 OK)**
```json
[
  {
    "id": 1,
    "title": "Arms and the Man",
    "author": "Darcy Bailey",
    "price": 39.90
  }
]
```
### **Headers (200 OK)**

| Chave    | Tipo   |  Descrição |
|--------------|---------|-----------|
| `X-Total-Pages`       | int     | Número total de páginas (baseado em zero) |
| `X-Total-Elements`    | int     | Total de dados encontrado no banco de dados |
| `X-Current-Page`      | int     | Página atual, o que foi informado no parâmetro page |
| `X-Page-Size`         | int     | Limite dos objetos que será retornado na atual página |

---

### **2. Buscar um livro por id**
**GET /books/{id}**

#### **Exemplo de requisição**
```http GET /books/1```

### **Exemplo de resposta (200 OK)**
```json
{
  "id": 1,
  "title": "1984",
  "author": "George Orwell",
  "price": 39.9,
  "types": [
    {
      "id": 1,
      "description": "Kindle Edition"
    }
  ],
  "mainGenres": [
    {
      "id": 12,
      "description": "Ficção Científica"
    }
  ],
  "subGenres": [
    {
      "id": 1,
      "description": "Distopia"
    }
  ]
}
```
### **Exemplo de resposta (404 Not Found)**
```json 
{
  "timestamp": "2025-05-07T08:00:00",
  "status": 404,
  "title": "Not Found",
  "detail": [
    "No item was found with the [ 555 ] entered."
  ],
  "type": "br.com.bookstore.exception.ResourceNotFoundException"
}
```

---

### **3. Buscar um livro por gênero (com paginação)**
**GET /books/genre/{genre}**
#### **Parâmetros de consulta (query params)**
| Parâmetro    | Tipo   | Obrigatório | Padrão | Mínimo | Máximo | Descrição |
|-------------|--------|-------------|---------|--------|--------|------------|
| `page`      | int    | Não         | 0       | 0      | -      | Número da página (baseado em zero) |
| `size`      | int    | Não         | 10      | 1      | 50    | Quantidade de itens por página |

#### **Parâmetros de consulta (path param)**
| Parâmetro   | Tipo   | Obrigatório | Descrição |
|-------------|--------|-------------|-----------|
| `genre`     | string | sim         | Gênero do livro a ser buscado |

#### **Exemplo de requisição**
```http GET /books/genre/romance?page=0&size=5```

### **Exemplo de resposta (200 OK)**
```json
[
  {
    "id": 2,
    "title": "To Kill a Mockingbird",
    "author": "Harper Lee",
    "price": 45.00
  }
]
```
### **Headers (200 OK)**

| Chave    | Tipo   |  Descrição |
|--------------|---------|-----------|
| `X-Total-Pages`       | int     | Número total de páginas (baseado em zero) |
| `X-Total-Elements`    | int     | Total de dados encontrado no banco de dados |
| `X-Current-Page`      | int     | Página atual, o que foi informado no parâmetro page |
| `X-Page-Size`         | int     | Limite dos objetos que será retornado na atual página |

---


### **4. Buscar um livro por autor (com paginação)**
**GET /books/author/{author}**
#### **Parâmetros de consulta (query params)**
| Parâmetro    | Tipo   | Obrigatório | Padrão | Mínimo | Máximo | Descrição |
|-------------|--------|-------------|---------|--------|--------|------------|
| `page`      | int    | Não         | 0       | 0      | -      | Número da página (baseado em zero) |
| `size`      | int    | Não         | 10      | 1      | 50    | Quantidade de itens por página |

#### **Parâmetros de consulta (path param)**
| Parâmetro   | Tipo   | Obrigatório | Descrição |
|-------------|--------|-------------|-----------|
| `author`     | string | sim         | Autor do livro a ser buscado |

#### **Exemplo de requisição**
```http GET /books/author/George Orwell?page=0&size=5```

### **Exemplo de resposta (200 OK)**
```json
[
  {
    "id": 1,
    "title": "1984",
    "author": "George Orwell",
    "price": 39.90
  }
]
```
### **Headers (200 OK)**

| Chave    | Tipo   |  Descrição |
|--------------|---------|-----------|
| `X-Total-Pages`       | int     | Número total de páginas (baseado em zero) |
| `X-Total-Elements`    | int     | Total de dados encontrado no banco de dados |
| `X-Current-Page`      | int     | Página atual, o que foi informado no parâmetro page |
| `X-Page-Size`         | int     | Limite dos objetos que será retornado na atual página |

---

### **5. Livros visto recentemente (implementar)**

**GET /books/recently-viewed**

#### **Exemplo de requisição**
```http GET /books/recently-viewed```

### **Exemplo de resposta (200 OK)**
```json
[
  {
    "id": 1,
    "title": "1984",
    "author": "George Orwell",
    "price": 39.90
  },
  {
    "id": 2,
    "title": "To Kill a Mockingbird",
    "author": "Harper Lee",
    "price": 45.00
  }
]
```

---

# I. Arquitetura de Solução e Arquitetura Técnica

A solução foi implementada utilizando Spring Boot com JDK 21, criando uma API RESTful para gerenciar informações de livros em uma livraria. A arquitetura segue os padrões modernos de desenvolvimento em microservices, com a aplicação dividida em camadas bem definidas: Controller, Service, e Repository.
- Spring Boot foi utilizado para estruturar a aplicação e facilitar a configuração e a criação de endpoints REST.
- JPA (Java Persistence API) foi utilizado para persistência de dados, e Hibernate como implementador, com suporte a banco de dados MySQL.
- Redis foi integrado para fornecer caching e melhorar a performance das consultas frequentes, como a listagem de livros e a busca por gênero ou autor.
- A aplicação utiliza Paginação (com PageRequest), permitindo consultas eficientes para exibir uma quantidade controlada de livros por vez.
- DTOs (Data Transfer Objects) são usados para abstrair as entidades de banco de dados e expor informações de maneira controlada através da API.

# Decisões de design

- A utilização de Redis como cache para listas e contagens de livros melhora a performance, reduzindo a carga sobre o banco de dados em consultas repetidas.
- A API foi projetada para ser simples, com endpoints claros para listar livros, buscar por ID, por gênero ou autor.
- A escolha do Spring Boot e JDK 21 proporciona uma configuração fácil e aproveita as vantagens do Java moderno, como melhor performance e maior segurança.

# II. Explicação sobre o Case Desenvolvido (Plano de Implementação)

A aplicação foi desenvolvida com foco em fornecer uma API eficiente e fácil de usar para consulta de livros. A estrutura básica envolve quatro endpoints principais para manipulação de dados de livros:

1. GET /books: Lista todos os livros com suporte a paginação.
2. GET /books/{id}: Recupera os detalhes de um livro específico, incluindo título, autor, preço e categorias (tipos, gêneros principais e subgêneros).
3. GET /books/genre/{genre}: Filtra livros pelo gênero principal.
4. GET /books/author/{author}: Filtra livros pelo nome do autor.

# Lógica do Negócio

- Serviço de livros (BookService): Contém as lógicas para buscar livros com base nos filtros aplicados, utilizando o repositório JPA para interagir com o banco de dados. A lógica de paginação e contagem de livros é centralizada nesta camada.
- Cache com Redis: Utiliza-se o Redis para armazenar em cache as listas de livros, reduzindo a necessidade de acessar o banco de dados para consultas repetidas. O caching é implementado usando a anotação @Cacheable do Spring.
- Mapeamento de Entidades para DTOs: O mapper é utilizado para converter as entidades do banco de dados (BookEntity) para os objetos de transferência de dados (DTOs, como Book e BookDetail), garantindo que apenas os dados necessários sejam expostos pela API.
- Tratamento de erros: A API lida com erros como recursos não encontrados, utilizando exceções personalizadas como ResourceNotFoundException para retornar respostas claras e padronizadas ao cliente.

# III. Melhorias e Considerações Finais

- Segurança: Acrescentaria uma camada de autenticação e autorização utilizando JWT para garantir que apenas usuários autenticados possam acessar os dados.
- Documentação: Acrescentaria mais exemplos detalhados para facilitar o uso por outros desenvolvedores.
- Testes: Criação de mais casos de teste.
- Cache: Encontrar uma melhor solução para recuperar os valores guardados no cache

# Desafios

- A configuração do cache com a lista de livros. 
- Configurar arquivo docker-compose para inicializar a aplicação na porta localhost:8080
- Implementação dos testes com redis (serviço de books por id) 

---

# Como rodar o projeto?

**Requisitos**

- [JDK 21 ou superior](https://www.oracle.com/java/technologies/javase/jdk22-archive-downloads.html).
- [Docker](https://www.docker.com/get-started/)
- [Git](https://git-scm.com/downloads)
- [Postman](https://www.postman.com/downloads/?%3Bpreview=true)

1. Clonar o repositório:

Abra o terminal do Windows e digite o comando abaixo:

```bash
git clone https://github.com/JeffersonXimenes/bookstore
```

Ou, caso não tenha o Git instalado em sua máquina, é possível fazer o download do arquivo ZIP da seguinte maneira:

![image](https://github.com/user-attachments/assets/a9532999-98a8-4a26-9114-42f7869056d2)

Após o download do ZIP, o arquivo deve ser extraído em algum diretório do seu computador.

2. Navegar até o diretório do projeto

```bash
cd bookstore
```

3. Rodar o comando do docker para subir os containers

```bash
docker-compose up -d
```

Caso queira derrubar os containers que subiram:

```bash
docker-compose down
```

Caso queira apagar todos os containers criados:

```bash
docker-compose down -v
```

4. Após rodado o comando do item 3 a aplicação ira subir em segundo plano

5. (Caso queira testar no postman) No projeto, tem uma pasta com nomenclatura postman! É possível importar a mesma no seu postman e consumir os serviços mencionados acima. Abaixo, foi inserido um exemplo de como realizar esse import

![image](https://github.com/user-attachments/assets/b9a9d67f-26fe-40b7-acd3-84cd7a18b099)

---

### Documentação da API

A documentação da API é gerada automaticamente usando o Swagger e pode ser acessada em:

[http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)

---

### Diagrama do banco de dados

![image](https://github.com/user-attachments/assets/6aacbe50-53cb-4fee-bbd1-df8552900e7a)

---

### Referências

- [Spring Boot DevTools](https://docs.spring.io/spring-boot/3.4.4/reference/using/devtools.html)
- [Spring Web](https://docs.spring.io/spring-boot/3.4.4/reference/web/servlet.html)
- [Spring Data Redis (Acesso+Driver)](https://docs.spring.io/spring-boot/3.4.4/reference/data/nosql.html#data.nosql.redis)
- [Spring Boot Testing](https://docs.spring.io/spring-boot/docs/3.4.4/reference/html/features.html#features.testing)

----

### Guias

Os guias abaixo, ilustram como implementar cenários feitos nesse desafio

- [Dados dos livro Bookdata](https://www.kaggle.com/datasets/chhavidhankhar11/amazon-books-dataset)
- [Comandos docker](https://www.hostinger.com.br/tutoriais/docker-cheat-sheet?utm_campaign=Generic-Tutorials-DSA|NT:Se|LO:BR-t4&utm_medium=ppc&gad_source=1&gad_campaignid=20417074995&gbraid=0AAAAADMy-hYKV-0tI3u4kbk_Gs8863XYT&gclid=Cj0KCQjwrPHABhCIARIsAFW2XBNtZ10BAFoZ6DS43rLfAEU82y9IXRZjUt0QLldNiX-cspM2hPQHVo4aAouoEALw_wcB)
- [1° Utilizando Redis para cache](https://medium.com/@tharindudulshanfdo/optimizing-spring-boot-applications-with-redis-caching-35eabadae012)
- [2° Utilizando Redis para cache](https://docs.spring.io/spring-data/redis/reference/redis/redis-cache.html)
- [1° Implementação Swagger](https://www.baeldung.com/swagger-set-example-description)
- [2° Implementação Swagger](https://www.baeldung.com/spring-rest-openapi-documentation)