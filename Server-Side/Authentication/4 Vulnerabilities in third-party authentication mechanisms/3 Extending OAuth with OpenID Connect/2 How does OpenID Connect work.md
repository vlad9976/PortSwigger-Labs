OpenID Connect slots neatly into the normal [OAuth flows](https://portswigger.net/web-security/oauth/grant-types). From the client application's perspective, the key difference is that there is an additional, standardized set of scopes that are the same for all providers, and an extra response type: `id_token`.

### OpenID Connect roles

The roles for OpenID Connect are essentially the same as for standard OAuth. The main difference is that the specification uses slightly different terminology.

- **Relying party** - The application that is requesting authentication of a user. This is synonymous with the OAuth client application.
- **End user** - The user who is being authenticated. This is synonymous with the OAuth resource owner.
- **OpenID provider** - An OAuth service that is configured to support OpenID Connect.

### OpenID Connect claims and scopes

The term "claims" refers to the `key:value` pairs that represent information about the user on the resource server. One example of a claim could be `"family_name":"Montoya"`.

Unlike basic OAuth, whose [scopes are unique to each provider](https://portswigger.net/web-security/oauth/grant-types#oauth-scopes), all OpenID Connect services use an identical set of scopes. In order to use OpenID Connect, the client application must specify the scope `openid` in the authorization request. They can then include one or more of the other standard scopes:

- `profile`
- `email`
- `address`
- `phone`

Each of these scopes corresponds to read access for a subset of claims about the user that are defined in the OpenID specification. For example, requesting the scope `openid profile` will grant the client application read access to a series of claims related to the user's identity, such as `family_name`, `given_name`, `birth_date`, and so on.

### ID token

The other main addition provided by OpenID Connect is the `id_token` response type. This returns a JSON web token (JWT) signed with a JSON web signature (JWS). The JWT payload contains a list of claims based on the scope that was initially requested. It also contains information about how and when the user was last authenticated by the OAuth service. The client application can use this to decide whether or not the user has been sufficiently authenticated.

The main benefit of using `id_token` is the reduced number of requests that need to be sent between the client application and the OAuth service, which could provide better performance overall. Instead of having to get an access token and then request the user data separately, the ID token containing this data is sent to the client application immediately after the user has authenticated themselves.

Rather than simply relying on a trusted channel, as happens in basic OAuth, the integrity of the data transmitted in an ID token is based on a JWT cryptographic signature. For this reason, the use of ID tokens may help protect against some man-in-the-middle attacks. However, given that the cryptographic keys for signature verification are transmitted over the same network channel (normally exposed on `/.well-known/jwks.json`), some attacks are still possible.

Note that multiple response types are supported by OAuth, so it's perfectly acceptable for a client application to send an authorization request with both a basic OAuth response type and OpenID Connect's `id_token` response type:

`response_type=id_token token response_type=id_token code`

In this case, both an ID token and either a code or access token will be sent to the client application at the same time.