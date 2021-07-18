```js
const { Company } = require('../companys/company');
const { sequelize, DataTypes, Sequelize } = require('../database');

/**
 * TABLE USER: 
 * id
 * userName
 * email
 * password
 * phone
 * role
 */
const User = sequelize.define('user', {
    id: {
        type: DataTypes.UUID,
        primaryKey: true,
        defaultValue: Sequelize.UUIDV4
    },
    userName: {
        type: DataTypes.STRING,
        allowNull: false,
    },
    email: {
        type: DataTypes.STRING,
        unique: true,
        allowNull: false
    },
    password: {
        type: DataTypes.STRING,
        allowNull: false
    },
    phone: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true
    }
});

User.hasOne(Company, {
    as: 'company',
    foreignKey: 'userId'
});

Company.belongsTo(User, {
    as: 'user',
    foreignKey: 'userId'
})

module.exports = {User};
```
