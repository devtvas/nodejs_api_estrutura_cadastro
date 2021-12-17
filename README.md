## RocketSeat

Link: https://www.youtube.com/watch?v=BN_8bCfVp88

Projeto API NodeJS + Express + Mongo
Introducao a estrutura de cadastro.

# Primeiros passos

```sh
$ mkdir api_node_estrutura_de_cadastro
$ cd api_node_estrutura_de_cadastro
$ code .
$ npm init 
$ yarn add express
$ yarn add body-parser
$ mkdir src
$ cd src
$ touch index.js
```

## editar arquivo src/index.js

```
const express = require('express');
const bodyParser = require('body-parser');

const app = express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

app.get('/', (req, res) => {
    res.send('Ok');
})

//definir a porta que eu quero ouvir:
app.listen(3000);

```
# Intalar as dependencias DataBase

```
$ yarn add mongoose
$ mkdir database
$ cd database
$ touch index.js
```

## editar arquivo src/database/index.js

```
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost/noderest', { useMongoClient: true});
mongoose.Promise = global.Promise;

module.exports = mongoose;

```

# Criar a Model

```

$ mkdir models
$ cd models
$ touch user.js
```

## editar arquivo src/models/user.js

```
const mongoose = require('mongoose');

const UserSchema = new mongoose.Schema({
    name: {
        type: String,
        require: true,
    },
    email: {
        type: String,
        unique: true,
        require: true,
        lowercase: true,
    },
    password: {
        type: String,
        require: true,
        secret: false,
    },
    createdAt: {
        type: Date,
        default: Date.now,
    },
});

const User = mongoose.model('User', UserSchema);

```

# Criar a Controllers

```

$ mkdir controllers
$ cd controllers
$ touch authController.js
```

## editar arquivo src/controllers/authController.js

```
const express = require("express");

const User = require('../models/User');

const router = express.Router();

router.post('/register', async (req, res) => {
    try {
        const user = await User.create(req.body);

        return res.send({ user });
    } catch (error) {
        return res.status(400).send({ error: 'Registration failed' });
    }
});

```
