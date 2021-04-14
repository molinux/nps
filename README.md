# 🌐 Net Promoter Score

### ❓ What a f*** is a Net Promoter Score?

O Net Promoter Score (or NPS) is a metric to measure customer satisfaction, 
through the calculation related votes to the question: 
"From 0 to 10, how much would you recommend our company ?"

Votes have a rating, which is taken into account, in this calculation:

| Vote | Classification |
| ---- | ----- |
De 0 a 6 | Detractor
De 7 a 8 | Passive
De 9 a 10 | Promoter

The classification notes are not consider on the NPS calculation.

The NPS formula is:

<code>(Promoter - Detractor) / Total of Avaliations</code>

<hr />

Through that application is possible to register
a user (who would be a costumer), create a survey,
list surveys, send emails to that costumers send 
their note and make the calculation of NPS.

## 💻 Endpoints

### 🧍 Users

<b> POST <code>/users</code> </b> Users creation

This endpoint must receive a name and an email in
the request body.

```node
// Request body
{
    name: "Molinux",
    email: "molinuxbr@gmail.com"
}
```

```node
// Return
{
    id: "39a3a2c9-1c33-415f-9ed0-3c8d6c3ba9f5",
    name: "Molinux",
    email: "molinuxbr@gmail.com",
    created_at: "2021-04-14T19:36:15.000Z"
}
```



### 🔎 Surveys

<b> GET <code>/surveys</code></b> Listar todas as pesquisas criadas

```node
// Return
[
  {
    id: "c6d8506b-9881-43d6-9024-62ed21978fc0",
    title: "Molinux Corp",
    description: "From 0 to 10, how much would you recommend Molinux Corp. ?",
    created_at: "2021-04-14T03:13:49.000Z"
  },
  {
    id: "1f7f097c-5906-421e-8c3f-477798468e6b",
    title: "X Corp.",
    description: "From 0 to 10, how much would you recommend X Corp. ?",
    created_at: "2021-04-14T03:17:27.000Z"
  }
]
```

<b>POST <code>/surveys</code></b> Criar uma nova pesquisa

Este endpoint precisa receber um título e uma descrição.

```node
// Corpo da requisição
{
    title: "Empresa X",
    description: "De 0 a 10, o quanto você recomenda a Empresa X?"
}
```

```node
// Retorno
{
    id: "1f7f097c-5906-421e-8c3f-477798468e6b",
    title: "Empresa X",
    description: "De 0 a 10, o quanto você recomenda a Empresa X?",
    created_at: "2021-02-26T03:17:27.000Z"
  }
```

### ✉️ Envio de Email

<b>POST <code>/sendMail</code></b> Envio de email contendo link para avaliação de empresa

Necessário conter um email válido e o id de uma pesquisa. 

```node
// Corpo da requisição
{
    email: "debora@gmail.com",
    survey_id: "1f7f097c-5906-421e-8c3f-477798468e6b"
}	
```

Caso o usuário esteja recebendo o email pela primeira vez,
o retorno será o seguinte:

```node
{
  id: "7df04a6a-fac6-443a-9249-c77363b42656",
  user_id: "39a3a2c9-1c33-415f-9ed0-3c8d6c3ba9f5",
  survey_id: "1f7f097c-5906-421e-8c3f-477798468e6b",
  created_at: "2021-02-28T19:44:14.000Z"
}
```

Caso já tenha recebido o email, mas ainda não tenha avaliado, será o seguinte:

```node
{
  id: "7df04a6a-fac6-443a-9249-c77363b42656",
  user_id: "39a3a2c9-1c33-415f-9ed0-3c8d6c3ba9f5",
  survey_id: "1f7f097c-5906-421e-8c3f-477798468e6b",
  value: null,
  created_at: "2021-02-28T19:44:14.000Z",
  user: {
    id: "39a3a2c9-1c33-415f-9ed0-3c8d6c3ba9f5",
    name: "Débora",
    email: "debora@gmail.com",
    created_at: "2021-02-28T19:36:15.000Z"
  },
  survey: {
    id: "1f7f097c-5906-421e-8c3f-477798468e6b",
    title: "Empresa X",
    description: "De 0 a 10, o quanto você recomenda a Empresa X?",
    created_at: "2021-02-26T03:17:27.000Z"
  }
}
```

### 🖩 Cálculo de NPS

<b>GET <code>/nps/:id_da_pesquisa</code></b> Cáculo de NPS, com base em avaliações de uma pesquisa

Para este endpoint, é necessário passar o id de uma pesquisa, na rota.

```node
// Retorno
{
  detractors: 2,
  promoters: 4,
  passives: 0,
  totalAnswers: 6,
  nps: 33.33
}
```

## 💿 Instalação

Para instalar este repositório, é necessário baixá-lo.

```bash
git clone https://github.com/deboralbarros/nlw4.git
```

Depois, basta entrar na pasta do projeto e instalar as dependências:

```node
cd nlw4

// instalando com yarn
yarn

// instalando com npm
npm install
```

Então, depois de instaladas as dependências, é necessário rodar as migrations:

```node
// com yarn
yarn typeorm migration:run

// com npm
npm run typeorm migration:run
```

Antes de rodar, é necessário criar um arquivo <code>.env</code> na raiz do projeto para que
a funcionalidade de avaliação funcione. Dentro deste arquivo, coloque:
```angular2html
URL_MAIL=http://localhost:3333/answers
```

E agora, para rodar, basta dar <code>yarn dev</code> que a aplicação estará rodando em [localhost:3333](http://localhost:3333).

## ⚙️ Tecnologias Utilizadas

* [Node.js](https://nodejs.org/en/)
* [Express](https://expressjs.com/pt-br/)
* [Typescript](https://www.typescriptlang.org/)
* [SQLite](https://www.sqlite.org/index.html)
* [TypeORM](https://typeorm.io/#/)
* [Nodemailer](https://nodemailer.com/about/)
* [Ethereal](https://ethereal.email/) 
* [Yup](https://github.com/jquense/yup)
* [Jest](https://jestjs.io/)
* [Handlebars](https://handlebarsjs.com/)