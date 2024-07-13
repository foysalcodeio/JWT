JWT
## backend - primary-step: 1
```
  app.post('/jwt', async (req, res) => {
        const user = req.body;
        console.log(user)
        res.send(user)
    });
```

## front-end - primary-step: 1
```
//get access jwt token from backend
const user = {email}
axios.post('http://localhost:5500/jwt', user)
.then(response => {
    console.log(response.data)
})
```
