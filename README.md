# Guia prático Express JS
Guia prático para iniciantes no Express JS, um framework minimalista para Node.js para construir aplicações web e APIs.

- Esta é a documentação oficial do Express JS: https://expressjs.com/


## Introdução
O Express JS é um framework web para Node.js que facilita a criação de aplicações web e APIs. Ele fornece uma série de recursos e funcionalidades que tornam o desenvolvimento mais rápido e eficiente.

Siga os passos abaixo para configurar um ambiente de desenvolvimento básico com Express JS. 

## Pre-requisitos
- Node.js instalado
    - Para instalar o Node.js, acesse https://nodejs.org/en/download/ e siga as instruções de instalação para o seu sistema operacional.
    - Para verificar se o Node.js está instalado, execute o comando `node -v` no terminal.

## Criando um novo projeto
1. Crie uma nova pasta e abra  usando o VS Code.
2. Abra o terminal integrado do VS Code (Ctrl + j).
3. Execute o comando `npm init -y` para criar um novo arquivo `package.json` com as configurações padrão.
4. Instale o Express JS executando o comando `npm install express`.

## Criando um servidor básico
1. Crie um novo arquivo chamado `app.js` na raiz do seu projeto.
2. Abra o arquivo `app.js` e adicione o seguinte código:
```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;
app.get('/', (req, res) => {
    res.send('Hello World!');
});
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```
### Explicação do código:
- `const express = require('express');`: Importa o módulo Express.
- `const app = express();`: Cria uma instância do aplicativo Express.
- `const PORT = process.env.PORT || 3000;`: Define a porta em que o servidor irá escutar. Se a variável de ambiente `PORT` não estiver definida, usará a porta 3000.
- `app.get('/', (req, res) => { ... });`: Define uma rota GET para a raiz do aplicativo. Quando um usuário acessar a URL raiz (`/`), o servidor responderá com "Hello World!".
- `app.listen(PORT, () => { ... });`: Inicia o servidor e faz com que ele escute na porta definida. Quando o servidor estiver rodando, ele exibirá uma mensagem no console informando a URL onde está acessível.

### Executando o servidor
Para executar o servidor, abra o terminal integrado do VS Code e execute o comando:

```bash 
node app.js
```
- O servidor será iniciado e você verá a mensagem "Server is running on http://localhost:3000" no console.
- Abra o navegador e acesse `http://localhost:3000`. Você deverá ver a mensagem "Hello World!" exibida na tela.



## Criando um CRUD básico
Para criar um CRUD (Create, Read, Update, Delete) básico com Express JS, você pode seguir os passos abaixo. Neste exemplo, vamos criar um CRUD simples para gerenciar uma lista de usuários.

### 1. Estrutura do Projeto
Crie a seguinte estrutura de pastas e arquivos:

```
my-express-app/
├── app.js
├── package.json
└── routes/
    └── users.js
```
### 2. Configurando o `app.js`
Abra o arquivo `app.js` e adicione o seguinte código:

```javascript
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;
const usersRouter = require('./routes/users');
app.use(express.json()); // Server para analisar JSON no corpo das requisições
app.use('/users', usersRouter); // Define a rota base para os usuários
app.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```
### 3. Criando o arquivo de rotas `users.js`
Abra o arquivo `routes/users.js` e adicione o seguinte código:

```javascript
const express = require('express');
const router = express.Router();
let users = []; // Array para armazenar os usuários
// Rota para criar um novo usuário
router.post('/', (req, res) => {
    const user = req.body;
    users.push(user);
    res.status(201).json(user);
});

// Rota para obter todos os usuários
router.get('/', (req, res) => {
    res.json(users);
});

// Rota para obter um usuário específico pelo ID
router.get('/:id', (req, res) => {
    const userId = parseInt(req.params.id);
    const user = users.find(u => u.id === userId);
    if (!user) {
        return res.status(404).json({ message: 'User not found' });
    }
    res.json(user);
});

// Rota para atualizar um usuário pelo ID
router.put('/:id', (req, res) => {
    const userId = parseInt(req.params.id);
    const userIndex = users.findIndex(u => u.id === userId);
    if (userIndex === -1) {
        return res.status(404).json({ message: 'User not found' });
    }
    const updatedUser = { ...users[userIndex], ...req.body };
    users[userIndex] = updatedUser;
    res.json(updatedUser);
});

// Rota para excluir um usuário pelo ID
router.delete('/:id', (req, res) => {
    const userId = parseInt(req.params.id);
    const userIndex = users.findIndex(u => u.id === userId);
    if (userIndex === -1) {
        return res.status(404).json({ message: 'User not found' });
    }
    users.splice(userIndex, 1);
    res.status(204).send(); // Retorna 204 No Content
});
module.exports = router;
```

- Ao executar o código acima, você terá um CRUD básico para gerenciar usuários. As rotas disponíveis são:
  - `POST /users`: Cria um novo usuário.
  - `GET /users`: Obtém todos os usuários.
  - `GET /users/:id`: Obtém um usuário específico pelo ID.
  - `PUT /users/:id`: Atualiza um usuário específico pelo ID.
  - `DELETE /users/:id`: Exclui um usuário específico pelo ID.

- Para iniciar o servidor, execute o comando `node app.js` no terminal. O servidor estará rodando em `http://localhost:3000`.