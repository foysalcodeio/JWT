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

```
    const verifyFBToken = (req, res, next) => {
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
      next();
    }
  
```
