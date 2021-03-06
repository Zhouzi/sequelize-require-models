# sequelize-require-models

Require all Sequelize models from a folder and associate them together.

## Installation

```
npm install sequelize-require-models --save
```

## Example

Given the following structure:

```
models/
    User.js
    Post.js
    index.js
```

Here's what the `index.js` file could contain:

```js
const Sequelize = require('sequelize');
const requireModels = require('sequelize-require-models');

const database = new Sequelize('database', 'root', 'password', {
    host: 'localhost',
    dialect: 'mysql'
});
const models = requireModels(database, __dirname);

module.exports = Object.assign({ database }, models);
```

Models can declare an "associate" function for associations.
For example, here's what `User.js` may contain:

```js
const Sequelize = require('sequelize');

function defineUser(database) {
    const User = database.define('user', {
        username: {
            type: Sequelize.STRING
        },
        password: {
            type: Sequelize.STRING
        }
    });
    User.associate = ({ Post }) => {
        User.hasMany(Post);
    };
    return User;
}

module.exports = defineUser;
```

## Documentation

### requireModels(database: Sequelize, folder: string)

* `database`: an instance of Sequelize
* `folder`: the folder to look into
