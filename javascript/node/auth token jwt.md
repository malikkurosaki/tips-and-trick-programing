```js
const jwt = require('jsonwebtoken');
const dotEnv = require('dotenv');
dotEnv.config();

class AuthToken{
    static generateAccessToken(id){
        
        return jwt.sign({id: id}, process.env.TOKEN_SECRET, { expiresIn: '9999 years' });
    }
    
    static authenticateToken(req, res, next) {
        const authHeader = req.headers['authorization']
        const token = authHeader && authHeader.split(' ')[1]
      
        if (token == null) return res.sendStatus(401);

        jwt.verify(token, process.env.TOKEN_SECRET, (err, id) => {
      
          if (err) return res.sendStatus(403)
      
          req.user = id;
      
          next()
        })
    }
}

module.exports = {AuthToken}
```
