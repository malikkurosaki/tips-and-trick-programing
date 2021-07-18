```js
const { Sequelize, DataTypes } = require('sequelize');
const { MySetting } = require('./my_setting');

const sequelize = new Sequelize('fb', 'root', 'Makuro_123', {
    host: 'localhost',
    dialect: 'mysql',
    logging: MySetting.isLogDb
});

;(async () => {
    try {
        await sequelize.authenticate();
        console.log('terkoneksi dengan database');
    } catch (error) {
        console.log('gagal terkoneksi dengan database');
    }
});

module.exports = { sequelize, Sequelize, DataTypes };
```
