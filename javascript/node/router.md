```js
const express = require('express');
const { AuthToken } = require('../auth_token');
const { UserController } = require('./user_controller');
const userRouter = express.Router();

const USER = '/user';
const GET_ALL_USER = '/getAllUser';
const LOGIN = '/login';

userRouter.get(USER, AuthToken.authenticateToken, UserController.get );
userRouter.post(USER, UserController.post );
userRouter.put(USER, AuthToken.authenticateToken, UserController.put );
userRouter.delete(USER, AuthToken.authenticateToken, UserController.delete );

userRouter.post(LOGIN, UserController.login );
userRouter.get(GET_ALL_USER, AuthToken.authenticateToken, UserController.getAllUser );

module.exports = { userRouter };

```
