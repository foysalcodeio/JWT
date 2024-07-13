JWT
## backend - primary-step: 1
```
  app.post('/jwt', async (req, res) => {
        const user = req.body;
        console.log(user)
        res.send(user)
    });
```

## front-end - primary-stage
```
//get access jwt token from backend
const user = {email}
axios.post('http://localhost:5500/jwt', user)
.then(response => {
    console.log(response.data)
})
```
# edit - 2nd stage
```
  app.post('/jwt', async (req, res) => {
        const user = req.body;
        console.log(user)
        const token = jwt.sign(user, 'secret', {expiresIn: '1h'}) // get token
        res.send(token)
    });
```
Automatic create a secret
```
require('crypto').randomBytes(64).toString('hex')
```
Working on Cookie parser
```
 npm install cookie-parser
```
