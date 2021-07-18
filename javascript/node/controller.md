```js
const expressAsyncHandler = require('express-async-handler');
const { AuthToken } = require('../auth_token');
const { User, Keys } = require('../association');
const { Company } = require('../companys/company');
const { Outlet } = require('../outlets/outlet');


class UserController{;

    static getAllUser = expressAsyncHandler (
        async (req, res) => {
            User.findAll({
                include: [
                    {all: true}
                ]
            })
            .then( data => {
                res.json({
                    success: true,
                    data: data
                })
            }).catch( error => {
                res.json({
                    success: false,
                    message: error
                });
            });
        }
    );

    static post = expressAsyncHandler(
        async (req, res) => {
            const body = req.body;
            await User.create(body, {
                include: [
                    {
                        model: Company,
                        as: 'company',
                        include: [
                            {
                                model: Outlet,
                                as: 'outlets'
                            }
                        ]
                    }
                ]
            })
            .then( data => {

                const _token = AuthToken.generateAccessToken(data.id);
                res.json({
                    success: true,
                    message: "user berhasil didaftarkan",
                    token: _token,
                    data: data
                })

            }).catch ( error => {
                res.json({
                    success: false,
                    message: 'error ' + error
                })
            }); 
        }
    )

    static login = expressAsyncHandler(
        async (req, res) => {
            const {email, password} = req.body;

            await User.findOne({
                where: {
                    email: email,
                    password: password
                },
                include: [
                    {
                        model: Company,
                        as: 'company',
                        include: [
                            {
                                model: Outlet,
                                as: 'outlets'
                            }
                        ]
                    },
                ]
            }).then( data => {
                const _success = data != null;

                const _token = _success? AuthToken.generateAccessToken(data.id): null;
                res.json({
                    success: _success? true: false,
                    message: _success? 'berhasil login': 'user tidak terdaftar silahkan register',
                    token: _token,
                    data: _success? data: null
                });
            }).catch( error => {
                res.json({
                    success: false,
                    message: error.message
                })
            });
        }
    )

    static get = expressAsyncHandler(
        async (req, res) => {
            const _id = req.user.id;

            const _result = await User.findOne({
                where: {
                    id: _id
                },
                include: {
                    association: User.Company,
                    include: [
                        {association: Company.Outlet}
                    ]
                }
            });

            const _success = _result != null;

            res.json({
                "success": _success,
                "data": _success? _result: null
            })
        }
    );

    static put = expressAsyncHandler(
        async (req, res) => {
            try {
                const id = req.user.id;
                const body = req.body;

                const result = await User.update( body, {
                    where: {
                        id: id
                    }
                });
                
                const _berhasil = result != 0;

                const _user = await User.findOne({
                    where: {
                        id: id
                    }
                });

                res.json({
                    "success": _berhasil,
                    "message": _berhasil? "berhasil update user": 'data tidak ada , tidak ada yang perlu di update',
                    "data": _user
                });

            } catch (error) {
                res.json({
                    "success": false,
                    "message": error.message
                })
            }
 
        }
    );

    static delete = expressAsyncHandler(
        async (req, res) => {

            const id = req.user.id;
            const result = await User.destroy({
                where: {
                    id: id
                }
            }).then( data => {
                const _success = data != 0;
                res.json({
                    success: _success,
                    message: _success? 'data berhasil di hapus': "data tidak terdapat didatabase"
                })
            }).catch( error => {
                res.json({
                    success: false,
                    message: error.message
                })
            });
        }
    );
}


module.exports = { UserController };
```
