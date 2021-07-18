# sequelize many-to-many relation

```js
const {Model, DataTypes, Sequelize} = require('sequelize');
const {sequelize} = require('./main/database');


const Pro = sequelize.define('pro', {
    nama: DataTypes.STRING
}, {timestamps: false});

const Ot = sequelize.define('ot', {
    nama: DataTypes.STRING
}, {timestamps: false});

const ProOt = sequelize.define('proOt', {
    isActive: {
        type: DataTypes.BOOLEAN,
        defaultValue: true
    },
    proId: DataTypes.INTEGER,
    otId: DataTypes.INTEGER
}, {timestamps: false});


Pro.belongsToMany(Ot, {through: ProOt, constraints: false});
Ot.belongsToMany(Pro, {through: ProOt, constraints: false});

;(async () => {

    // await sequelize.query('SET FOREIGN_KEY_CHECKS = 0');

    // await Pro.sync({force: true});
    // await Ot.sync({force: true});
    // await ProOt.sync({force: true});

    const proVal = Array.from(Array(10), (x, i) => {
        return {
            nama: "malik" + (i + 2)
        }
    });

    const otVal = Array.from(Array(10), (x, i) => {
        return {
            nama: "outlet" + (i + 2)
        }
    });

    const proOtVal = Array.from(Array(10), (x, i) => {
        return {
            proId: 2,
            otId: i + 2
        }
    });
    

    // const pro = await Pro.bulkCreate(proVal);
    // const ot = await Ot.bulkCreate(otVal);
    const proOt = await ProOt.bulkCreate(proOtVal, {updateOnDuplicate: ['proId']});

    const temu = await Pro.findAll({include: Ot, where: {id: 2}});

    console.log(JSON.stringify(temu, null, 2));
    

    // await sequelize.query('SET FOREIGN_KEY_CHECKS = 1');
})();

```
