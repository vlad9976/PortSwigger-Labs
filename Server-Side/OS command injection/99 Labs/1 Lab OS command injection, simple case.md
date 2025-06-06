
Solution

1. Use Burp Suite to intercept and modify a request that checks the stock level.
2. Modify the `storeID` parameter, giving it the value `1|whoami`.
3. Observe that the response contains the name of the current user.