```js
const express = require('express');
const cors = require('cors');
const app = express();
app.use(cors());
app.use(express.json());
app.use(express.urlencoded({extended: false}));

const { userRouter } = require('./users/user_router');
const { homeRouter } = require('./home/home_router');
const { companyRouter } = require('./companys/company_router');
const { categoryRouter } = require('./categorys/category_router');
const { outletRouter } = require('./outlets/outlet_router');
const { customerRouter } = require('./customers/customer_router');
const { typeOrderRouter } = require('./type_order/type_order_router');
const { productRouter } = require('./products/product_router');
const { employeeRouter } = require('./employees/employee_router');


app.use(homeRouter);
app.use('/api', userRouter);
app.use('/api', companyRouter);
app.use('/api', categoryRouter);
app.use('/api', outletRouter);
app.use('/api', customerRouter);
app.use('/api', typeOrderRouter);
app.use('/api', productRouter);
app.use('/api', employeeRouter);


app.get('*', (req, res) => {
    res.status(404).json({
        status: false,
        message: 'OH NO | 404'
    });
});
app.listen(3000, () => console.log('app run on port 3000'));
module.exports = {app};


```
