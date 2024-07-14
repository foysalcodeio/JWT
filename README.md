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
## edit - 2nd stage
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
# Working on Cookie parser
## installation command
```
 npm install cookie-parser
```
## cors - permission and access token
```
app.use(cors({
  origin: ['http://localhost:5173'],
  credentials: true
}));
app.use(express.json());
```
## Generating cookie in backend & send cookie client side and fixed browser
```
app.post('/jwt', async (req, res) => {
        const user = req.body;
        console.log(user)
        const token = jwt.sign(user, process.env.ACCESS_TOKEN_SECRET, {expiresIn: '1h'})
        res
        .cookie('token', token, {
          httpOnly: true,
          secure: false, // when i use for production then i use or turn - secure-true
          sameSite: 'none', // client and server is not same site
        })
        .send({success: true})
    });
```
## client site axios setting & get access jwt token from backend
```
 const user = {email}
  axios.post('http://localhost:5500/jwt', user, {withCredentials: true})
  .then(response => {
      console.log('response data -', response.data)
  })
```
## taking cookie from backend and again cookie send server
cart.jsx
```
    useEffect(() => {
        if (user?.email) {
            axios.get(url, {withCredentials: true})
                .then(response => {
                    console.log(response.data);
                    setBookings(response.data);
                })
                .catch(error => {
                    console.error('Error fetching bookings:', error);
                });
        }
    }, [url, user?.email]);
```


