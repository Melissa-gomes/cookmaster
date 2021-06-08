# Repositório do projeto Cookmaster!
---

# Sumário


  - [O que foi desenvolvido](#o-que-será-ser-desenvolvido)
  - [Desenvolvimento](#desenvolvimento)
  - [Conexão com o Banco](#conexão-com-o-banco)
  - [Coleções](#Coleções)
  - [Testes](#testes)
      - [1 - Endpoint para o cadastro de usuários](#1---crie-um-endpoint-para-o-cadastro-de-usuários)
      - [2 - Endpoint para o login de usuários](#2---crie-um-endpoint-para-o-login-de-usuários)
      - [3 - Endpoint para o cadastro de receitas](#3---crie-um-endpoint-para-o-cadastro-de-receitas)
      - [4 - Endpoint para a listagem de receitas](#4---crie-um-endpoint-para-a-listagem-de-receitas)
      - [5 - Endpoint para visualizar uma receita específica](#5---crie-um-endpoint-para-visualizar-uma-receita-específica)
      - [6 - Permissões do usuário admin](#6---permissões-do-usuário-admin)
      - [7 - Endpoint para a edição de uma receita](#7---crie-um-endpoint-para-a-edição-de-uma-receita)
      - [8 - Endpoint para a exclusão de uma receita](#8---crie-um-endpoint-para-a-exclusão-de-uma-receita)

---

# Habilidades

Neste projeto, as habilidades ultilizadas para a realização foram:

- A criação de tokens a partir de informações como login e senha;

- Autenticação de rotas do Express, usando o token JWT;

- Salvar arquivos no servidor através de uma API REST;

- Consultar arquivos do servidor através de uma api REST.

---

## O que foi desenvolvido

Foi desenvolvido um app utilizando a arquitetura MSC!

Neste novo projeto é possível fazer o cadastro e login de pessoa usuária, onde apenas esse usúario poderá acessar, modificar e deletar as receitas que cadastrou.

---

## Desenvolvimento

Desenvolvi todas as camadas da aplicação (Models, Service e Controllers)

Nessa aplicação, é possível realizar as operações básicas que se pode fazer em um determinado banco de dados: Criação, Leitura, Atualização e Exclusão (ou `CRUD`, pros mais íntimos).

Para realizar qualquer tipo de alteração no banco de dados (como cadastro, edição ou exclusão de receitas) é necessário autenticar-se pois os clientes apenas poderão disparar ações nas receitas que ele mesmo criou.

A autenticação é feita via `JWT`.


## Conexão com o Banco

A conexão do banco local contém os seguintes parâmetros:

```javascript
const MONGO_DB_URL = 'mongodb://localhost:27017/Cookmaster';
const DB_NAME = 'Cookmaster';
```

## Coleções

O banco tem duas coleções: usuários e receitas.

A coleção de usuários tem o seguinte nome: `users`.

Os campos da coleção `users` possuem este formato:

#### exemplo:

```json
{ "name" : "Erick Jacquin", "email" : "erickjacquin@gmail.com", "password" : "12345678", "role" : "user" }
```

A resposta do insert retornado após a criação é esta:

```json
{ "_id" : ObjectId("5f46914677df66035f61a355"), "name" : "Erick Jacquin", "email" : "erickjacquin@gmail.com", "password" : "12345678", "role" : "user" }
```
(O _id é gerado automaticamente pelo mongodb)

A coleção de receitas tem o seguinte nome: `recipes`.

Os campos da coleção `recipes` possuem este formato:

#### exemplo:

```json
{ "name" : "Receita do Jacquin", "ingredients" : "Frango", "preparation" : "10 minutos no forno" }
```

A resposta do insert retornado após a criação é esta:

```json
{ "_id" : ObjectId("5f46919477df66035f61a356"), "name" : "string", "ingredients" : "string", "preparation" : "string", "userId" : ObjectId("5f46914677df66035f61a355") }
```
(O _id é gerado automaticamente pelo mongodb, e o userId é gerado com o id do usuário que criou a receita)

---

## Testes

Todos as funcionalidades do projeto são testadss **automaticamente**. Cada `endpoint` possui vários requisitos e os testes para cada requisito de um `endpoint` estão no arquivo de teste correspondente.

_**Por exemplo**: Os requisitos relacionados ao `endpoint` `/users` estão no arquivo `users.test.js`._

Para executar os testes localmente, digite no terminal o comando `npm test`.

# Funcionalidades do projeto


### 1 - Endpoint para o cadastro de usuários

- A rota é (`/users`).



- O body da requisição possue o seguinte formato:

  ```json
  {
    "name": "string",
    "email": "string",
    "password": "string"
  }
  ```

**Além disso, as seguintes verificações são feitas:**

- **[É validado que o campo "name" é obrigatório]**

Se o usuário não tiver o campo "name" o resultado retornado será conforme exibido abaixo, com um status http `400`:

![Usuário sem Nome](./public/usuariosemnome.png)

- **[É validado que o campo "email" é obrigatório]**

Se o usuário não tiver o campo "email" o resultado retornado será conforme exibido abaixo, com um status http `400`:

![Usuário sem Email](./public/usuariosememail.png)

- **[É validado que não é possível cadastrar usuário com o campo email inválido]**

Se o usuário tiver o campo email inválido o resultado retornado será conforme exibido abaixo, com um status http `400`:

![Email Inválido](./public/campoemailinvalido.png)

- **[É validado que o campo "senha" é obrigatório]**

Se o usuário não tiver o campo "senha" o resultado retornado será conforme exibido abaixo, com um status http `400`:

![Usuário sem Senha](./public/usuariosemsenha.png)

- **[É validado que o campo "email" é único]**

Se o usuário cadastrar o campo "email" com um email que já existe, o resultado retornado será conforme exibido abaixo, com um status http `409`:

![Email já Usado](./public/emailjausado.png)

- **[É validado que é possível cadastrar usuário com sucesso]**

Se o usuário for cadastrado com sucesso o resultado retornado será conforme exibido abaixo, com um status http `201`:

![Usuário Cadastrado](./public/usuariocriadocomsucesso.png)

- **[É validado que é possível ao cadastrar usuário, o valor do campo "role" tenha o valor "user"]**

Se o usuário for criado com sucesso o resultado retornado será conforme exibido abaixo, com um status http `201`:

![Campo Role](./public/validarrole.png)

### 2 - Endpoint para o login de usuários

- A rota é (`/login`).

- O body da requisição possue o seguinte formato:

  ```json
  {
    "email": "string",
    "password": "string"
  }
  ```

**Além disso, as seguintes verificações foram feitas:**

- **[É validado que o campo "email" é obrigatório]**

Se o login não tiver o campo "email" o resultado retornado será conforme exibido abaixo, com um status http `401`:

![Usuário sem Senha](./public/loginsememail.png)

- **[É validado que o campo "password" é obrigatório]**

Se o login não tiver o campo "password" o resultado retornado será conforme exibido abaixo, com um status http `401`:

![Usuário sem Senha](./public/loginsemsenha.png)

- **[É validado que não é possível fazer login com um email inválido]**

Se o login tiver o email inválido o resultado retornado será conforme exibido abaixo, com um status http `401`:

![Email Inválido](./public/loginemailinvalido.png)

- **[É validado que não é possível fazer login com uma senha inválida]**

Se o login tiver a senha inválida o resultado retornado será conforme exibido abaixo, com um status http `401`:

![Senha Inválida](./public/loginsenhainvalida.png)

- **[É validado que é possível fazer login com sucesso]**

Se foi feito login com sucesso o resultado retornado será ser conforme exibido abaixo, com um status http `200`:

![Login com Sucesso](./public/logincomsucesso.png)

### 3 - Endpoint para o cadastro de receitas

- A rota é (`/recipes`).


- Nome, ingredientes e modo de preparo são recebidos no corpo da requisição, com o seguinte formato:

  ```json
  {
    "name": "string",
    "ingredients": "string",
    "preparation": "string"
  }
  ```


**Além disso, as seguintes verificações foram feitas:**

- **[É validado que não é possível cadastrar receita sem o campo "name"]**

Se a receita não tiver o campo "name" o resultado retornado será conforme exibido abaixo, com um status http `400`:

![Receita sem nome](./public/receitasemnome.png)

- **[É validado que não é possível cadastrar receita sem o campo "ingredients"]**

Se a receita não tiver o campo "ingredients" o resultado retornado será  conforme exibido abaixo, com um status http `400`:

![Receita sem ingrediente](./public/receitasemingrediente.png)

- **[É validado que não é possível cadastrar receita sem o campo "preparation"]**

Se a receita não tiver o campo "preparation" o resultado retornado será conforme exibido abaixo, com um status http `400`:

![Receita sem preparo](./public/receitasempreparo.png)

- **[É validado que não é possível cadastrar uma receita com token invalido]**

Se a receita não tiver o token válido o resultado retornado será conforme exibido abaixo, com um status http `401`:

![Receita com token inválido](./public/tokeninvalidoreq3.png)

- **[É validado que é possível cadastrar uma receita com sucesso]**

O resultado retornado para cadastrar a receita com sucesso será conforme exibido abaixo, com um status http `201`:

![Receita com Sucesso](./public/receitacomsucesso.png)

### 4 - Endpoint para a listagem de receitas

- A rota é (`/recipes`).

- A rota pode ser acessada por usuários logados ou não

**Além disso, as seguintes verificações foram feitas:**

- **[É validado que é possível listar todas as receitas sem estar autenticado]**

O resultado retornado para listar receitas com sucesso será conforme exibido abaixo, com um status http `200`:

![Receita com Sucesso](./public/listarreceitas.png)

- **[É validado que é possível listar todas as receitas estando autenticado]**

O resultado retornado para listar receitas com sucesso será conforme exibido abaixo, com um status http `200`:

![Receita com Sucesso](./public/listarreceitas.png)

### 5 - Crie um endpoint para visualizar uma receita específica

- A rota é (`/recipes/:id`).

- A rota pode ser acessada por usuários logados ou não

**Além disso, as seguintes verificações foram feitas:**

- **[É validado que é possível listar uma receita específica sem estar autenticado]**

O resultado retornado para listar uma receita com sucesso será conforme exibido abaixo, com um status http `200`:

![Listar uma Receita](./public/listarumareceita.png)

- **[É validado que é possível listar uma receita específica estando autenticado]**

O resultado retornado para listar uma receita com sucesso será conforme exibido abaixo, com um status http `200`:

![Listar uma Receita](./public/listarumareceita.png)

- **[É validado que não é possível listar uma receita que não existe]**

O resultado retornado para listar uma receita que não existe será conforme exibido abaixo, com um status http `404`:

![Listar uma Receita inexistente](./public/receitanaoencontrada.png)

### 6 - Permissões do usuário admin

Criei um arquivo `seed.js` na raiz do projeto com uma query do Mongo DB capaz de inserir um usuário na coleção _users_ com os seguintes valores:

`{ name: 'admin', email: 'root@email.com', password: 'admin', role: 'admin' }`

**Obs.:** Esse usuário caso criado tem o poder de criar, deletar, atualizar ou remover qualquer receita, independente de quem a cadastrou. Isso será solicitado ao longo dos próximos requisitos.

**Além disso, as seguintes verificações foram feitas:**

- **[É validado que o projeto tem um arquivo de seed, com um comando para inserir um usuário root e verifico que é possivel fazer login]**    

É validado que no arquivo `seed.js` existe a query para criar um usuário root

### 7 - Endpoint para a edição de uma receita

- A rota é (`/recipes/:id`).

- A receita só pode ser atualizada caso o usuário esteja logado e o token `JWT` validado.

- A receita só pode ser atualizada caso pertença ao usuário logado, ou caso esse usuário seja um admin.

- O body da requisição possue o seguinte formato:

  ```json
  {
    "name": "string",
    "ingredients": "string",
    "preparation": "string"
  }
  ```

**Além disso, as seguintes verificações foram feitas:**

- **[É validado que não é possível editar receita sem estar autenticado]**

O resultado retornado para editar receita sem autenticação será conforme exibido abaixo, com um status http `401`:

![Editar uma Receita sem autenticação](./public/editarsemautenticacao.png)

- **[É validado que não é possível editar receita com token inválido]**

O resultado retornado para editar receita com token inválido será conforme exibido abaixo, com um status http `401`:

![Editar uma Receita com token inválido](./public/editartokeninvalido.png)

- **[É validado que é possível editar receita estando autenticado]**

O resultado retornado para editar uma receita com sucesso será conforme exibido abaixo, com um status http `200`:

![Editar uma Receita](./public/editarcomsucesso.png)

- **[É validado que é possível editar receita com usuário admin]**

O resultado retornado para editar uma receita com sucesso será conforme exibido abaixo, com um status http `200`:

![Editar uma Receita](./public/editarcomsucesso.png)

### 8 - Endpoint para a exclusão de uma receita

- A rota é (`/recipes/:id`).

- A receita só pode ser excluída caso o usuário esteja logado e o token `JWT` validado.

- A receita só pode ser excluída caso pertença ao usuário logado, ou caso o usuário logado seja um admin.

**Além disso, as seguintes verificações foram feitas:**

- **[É validado que não é possível excluir receita sem estar autenticado]**

O resultado retornado para excluir uma receita sem autenticação será conforme exibido abaixo, com um status http `401`:

![Excluir uma Receita sem autenticação](./public/excluirsemautenticacao.png)

- **[É validado que é possível excluir receita estando autenticado]**

O resultado retornado para excluir uma receita com sucesso será conforme exibido abaixo, com um status http `204`:

![Excluir uma Receita](./public/excluircomsucesso.png)

- **[É validado que é possível excluir receita com usuário admin]**

O resultado retornado para excluir uma receita com sucesso será conforme exibido abaixo, com um status http `204`:

![Excluir uma Receita](./public/excluircomsucesso.png)


