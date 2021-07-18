```js
const { sequelize, DataTypes, Sequelize } = require("../database");

/**
 * Product
 * id, uuid, name, sellingPrice, isAllOutlet, isTypeOrder, listTypeorder, image, 
 */
const Product = sequelize.define('product', {
    id: {
        type: DataTypes.UUID,
        primaryKey: true,
        defaultValue: Sequelize.UUIDV4
    },
    name: {
        type: DataTypes.STRING,
        unique: true,
        allowNull: false
    },
    price: DataTypes.DECIMAL(14,2),
    isTypeOrder: {
        type: DataTypes.BOOLEAN,
        defaultValue: false
    },
    isAllOutlet: {
        type: DataTypes.BOOLEAN,
        defaultValue: true
    },
    isStock: {
        type: DataTypes.BOOLEAN,
        defaultValue: false
    },
    capitalPrice: DataTypes.DECIMAL(14,2),
    image: DataTypes.STRING,
    description: DataTypes.STRING,
    category: DataTypes.STRING,
    sku: DataTypes.STRING,
    barcode: DataTypes.STRING,
    curentStock: DataTypes.INTEGER,
    minimStock: DataTypes.INTEGER,
    note: DataTypes.STRING
});

/**
 * ProductOutlet
 * id
 * name
 * isActive
 */
const ProductOutlet = sequelize.define('productOutlet', {
    id: {
        type: DataTypes.UUID,
        primaryKey: true,
        defaultValue: Sequelize.UUIDV4
    },
    name: DataTypes.STRING,
    isActive: {
        type: DataTypes.BOOLEAN,
        defaultValue: true
    }
});

/**
 * ProductTypeOrder
 * id
 * name
 * price
 */
const ProductTypeOrder = sequelize.define('productTypeOrder', {
    id: {
        type: DataTypes.UUID,
        primaryKey: true,
        defaultValue: Sequelize.UUIDV4
    },
    name: DataTypes.STRING,
    price: {
        type: DataTypes.DECIMAL(14,2),
        defaultValue: 0
    }
});

/**
 * ProductStock
 * id
 * cusrentStock
 * minStock
 */
const ProductStock = sequelize.define('productStock', {
    id: {
        type: DataTypes.UUID,
        primaryKey: true,
        defaultValue: Sequelize.UUIDV4
    },
    curentStock: {
        type: DataTypes.INTEGER,
        defaultValue: 0
    },
    minStock: {
        type: DataTypes.INTEGER,
        defaultValue: 0
    }
});

Product.hasMany(ProductTypeOrder, {
    as: 'typeOrders',
    foreignKey: 'productId',
    hooks: true,
    onDelete: 'cascade'
});
ProductTypeOrder.belongsTo(Product, {
    as: 'product',
    foreignKey: 'productId'
});

Product.hasMany(ProductOutlet, {
    as: 'outlets',
    foreignKey: 'productId',
    hooks: true,
    onDelete: 'cascade'
});
ProductOutlet.belongsTo(Product, {
    as: 'product',
    foreignKey: 'productId'
});

Product.hasOne(ProductStock, {
    as: 'stock',
    foreignKey: 'productId',
    hooks: true,
    onDelete: 'cascade'
});
ProductStock.belongsTo(Product, {
    as: 'product',
    foreignKey: 'productId'
});

module.exports = {Product, ProductOutlet, ProductTypeOrder, ProductStock};
```
