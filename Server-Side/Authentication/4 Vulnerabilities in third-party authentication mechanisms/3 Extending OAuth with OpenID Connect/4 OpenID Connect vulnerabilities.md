
The specification for OpenID Connect is much stricter than that of basic OAuth, which means there is generally less potential for quirky implementations with glaring vulnerabilities. That said, as it is just a layer that sits on top of OAuth, the client application or OAuth service may still be vulnerable to some of the OAuth-based attacks we looked at earlier. In fact, you might have noticed that all of our [OAuth authentication labs](https://portswigger.net/web-security/all-labs#oauth-authentication) also use OpenID Connect.

In this section, we'll look at some additional vulnerabilities that may be introduced by some of the extra features of OpenID Connect.

### Unprotected dynamic client registration

The OpenID specification outlines a standardized way of allowing client applications to register with the OpenID provider. If dynamic client registration is supported, the client application can register itself by sending a `POST` request to a dedicated `/registration` endpoint. The name of this endpoint is usually provided in the configuration file and documentation.

In the request body, the client application submits key information about itself in JSON format. For example, it will often be required to include an array of whitelisted redirect URIs. It can also submit a range of additional information, such as the names of the endpoints they want to expose, a name for their application, and so on. A typical registration request may look something like this:

`POST /openid/register HTTP/1.1 Content-Type: application/json Accept: application/json Host: oauth-authorization-server.com Authorization: Bearer ab12cd34ef56gh89 { "application_type": "web", "redirect_uris": [ "https://client-app.com/callback", "https://client-app.com/callback2" ], "client_name": "My Application", "logo_uri": "https://client-app.com/logo.png", "token_endpoint_auth_method": "client_secret_basic", "jwks_uri": "https://client-app.com/my_public_keys.jwks", "userinfo_encrypted_response_alg": "RSA1_5", "userinfo_encrypted_response_enc": "A128CBC-HS256", … }`

The OpenID provider should require the client application to authenticate itself. In the example above, they're using an HTTP bearer token. However, some providers will allow dynamic client registration without any authentication, which enables an attacker to register their own malicious client application. This can have various consequences depending on how the values of these attacker-controllable properties are used.

For example, you may have noticed that some of these properties can be provided as URIs. If any of these are accessed by the OpenID provider, this can potentially lead to second-order [SSRF](https://portswigger.net/web-security/ssrf) vulnerabilities unless additional security measures are in place.