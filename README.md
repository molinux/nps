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

<b>POST <code>/surveys</code></b> To create a new survey

This endpoint needs to receive a title and a description.

```node
// Request body
{
    title: "X Corp.",
    description: "From 0 to 10, how much would you recommend X Corp. ?"
}
```

```node
// Return
{
    id: "1f7f097c-5906-421e-8c3f-477798468e6b",
    title: "X Corp.",
    description: "From 0 to 10, how much would you recommend X Corp. ?"
    created_at: "2021-04-14T03:17:27.000Z"
  }
```

### ✉️ Email sending

<b>POST <code>/sendMail</code></b> Sending an email containing a link to evalute the company

It is necessary to contain a valid email and a search id.

```node
// Request body
{
    email: "molinuxbr@gmail.com",
    survey_id: "1f7f097c-5906-421e-8c3f-477798468e6b"
}	
```

If the user is receiving the email for the first time,
the return will be as the follows:

```node
{
  id: "7df04a6a-fac6-443a-9249-c77363b42656",
  user_id: "39a3a2c9-1c33-415f-9ed0-3c8d6c3ba9f5",
  survey_id: "1f7f097c-5906-421e-8c3f-477798468e6b",
  created_at: "2021-04-14T19:44:14.000Z"
}
```

If the user has already received the email, but have not yet evaluated, it will be the following:

```node
{
  id: "7df04a6a-fac6-443a-9249-c77363b42656",
  user_id: "39a3a2c9-1c33-415f-9ed0-3c8d6c3ba9f5",
  survey_id: "1f7f097c-5906-421e-8c3f-477798468e6b",
  value: null,
  created_at: "2021-04-14T19:44:14.000Z",
  user: {
    id: "39a3a2c9-1c33-415f-9ed0-3c8d6c3ba9f5",
    name: "Molinux",
    email: "molinuxbr@gmail.com",
    created_at: "2021-04-14T19:36:15.000Z"
  },
  survey: {
    id: "1f7f097c-5906-421e-8c3f-477798468e6b",
    title: "X Corp.",
    description: "From 0 to 10, how much would you recommend X Corp. ?",
    created_at: "2021-02-26T03:17:27.000Z"
  }
}
```

### 🖩 NPS Calculation

<b>GET <code>/nps/:survey_id</code></b> NPS calculation, based on evaluations of a survey

For this endpoint, is necessary to inform a survey id on the route.

```node
// Return
{
  detractors: 2,
  promoters: 4,
  passives: 0,
  totalAnswers: 6,
  nps: 33.33
}
```

## 💿 Instalation

To install this repository, you need to download it.

```bash
git clone https://github.com/molinux/nps.git
```

And them, you just need to install dependencies on de project folder:

```node
cd nps

// install with yarn
yarn

//  install with npm
npm install
```

After installed the dependencies, you need to run the migrations:

```node
// with yarn
yarn typeorm migration:run

// with npm
npm run typeorm migration:run
```

Before running, it is necessary to create a <code>.env</code> file at the root of the project
so that the evaluation functionality works. Inside this file, place:

```angular2html
URL_MAIL=http://localhost:3333/answers
```

And now, to run, just give <code>yarn dev</code> and the application will be running in [localhost:3333](http://localhost:3333).

## ⚙️ Technologies used in this project

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