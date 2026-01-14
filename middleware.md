This is client side middleware where i everywhere intercept accessToken
```
const useAxiosSecure = () => {
    const {user} = useAuth();
    axiosSecure.interceptors.request.use((config) => {
        // JWT token SECTION AND axios interceptor
        config.headers.Authorization = `Bearer ${user?.accessToken}`; // this section is JWT token attach and all user token generated after login
        return config;
    }, error => {
        return Promise.reject(error);
    })
    return axiosSecure;
};

```
In backend any directory we check data response

```
console.log("Fetching payments jwt testing :", req.body);
console.log("Fetching payments jwt testing :", req.headers);
```
After that  we trying to catch everywhere in function to test data but big problem is large application is huge time so that we can add middleware where as every function to test it

সবার উপরের যে কোডটা আছে ওইটা একটা ক্লায়েন্ট সাইডে টোকেন জেনারেট করবে useAxiosSecure.post(.......) এর মাধ্যমে ব্যাকএন্ড ডাটাবেজ এ পাঠাবে যদি মিডলওয়্যার বা রিসিভ করার ফাংশন থাকে। এই ফাংশনকে আমরা verifyFirebaseToken নামে চিনি। এই verifyFirebaseToken client side থেকে আগত কোড চেক করে দেখবে নিদিষ্ট ইউজার থেকে প্রাপ্ত কোড ডাটা এক্সেস করার জন্য ঠিক আছে কিনা।

```
    const verifyFBToken = async (req, res, next) => {
      const authHeader = req.headers.authorization;
      if (!authHeader) {
        return res.status(401).send({ message: 'Unauthorized access: No token provided' });
      }
      const token = authHeader.split(' ')[1];
      if(!token) {
        return  res.status(401).send({ message: 'Unauthorized access: Malformed token' });
      }
      // token verification logic here
      // verify token with Firebase Admin SDK or other method
      try{
        const decoded = await admin.auth().verifyIdToken(token);
        req.decoded = decoded;
        next();
      }
      catch(error){
        return res.status(401).send({ message: 'Unauthorized access: Invalid token' });
      }
    }
  
```
ইউজার ইমেইল সাথে ডিকোডার ইউজার মেইল মিল থাকলে ডাটা দেখাবে তা না হলে দেখাবে না
```
     app.get("/payments", verifyFBToken, async (req, res) => {
      try {
        ..........................
        ..........................
        if(req.decoded.email !== userEmail){
          return res.status(403).send({ message: 'Forbidden access' });
        }
        ..........................
        ..........................
    });
```
