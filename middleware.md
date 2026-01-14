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
