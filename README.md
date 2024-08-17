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
## cors - permission and access token - cookies goes to client site
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
মিডলওয়্যার এ তিনটি জিনিস থাকে req, res এবং next । কোন একটি ডিরেক্টরী আমাদের সিকিউর করতে হলে র্সাভার সাইডে cookie-parser and middleware এবং ক্লায়েন্ট সাইডে withcredentials or credentials true এড করতে হবে তা না হলে ইরর আসবে। তারপর আমরা টোকেন জেনারেট করে টেস্ট করবো মাঝে মধ্যে এই ধরনের- [Object: null prototype] {}  ইরর আসতে পারে সে-ক্ষেত্রে ইউআরএল পাশে  fetch(url, {credentials: 'include'}) বসিয়ে দিতে হবে যদি আমরা  fetch ব্যবহার করি।
সিকিউর করার কিছু বিষয় জেনে নেই
1. verifytoken এর মাধ্যমে ডাটা চেক করব তবে এটা মিডিলওয়্যার ব্যবহার করব
2. মিডিলওয়্যারটি অবশ্যই ম্যানুয়াল হবে।
3. এটি আমরা নিদিষ্ট ফাংশনের মধ্যে ব্যবহার করব না  এটি সব জায়গাতে মানে গ্লোবালি ব্যবহার করব যেন সবজায়গা থেকে কল করা যায়।
4. 



