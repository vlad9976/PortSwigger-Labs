
Solution


1. Use Burp Suite to intercept and modify a request that fetches a product image.
2. Modify the `filename` parameter, giving it the value:
    
    `../../../etc/passwd`
3. Observe that the response contains the contents of the `/etc/passwd` file.
