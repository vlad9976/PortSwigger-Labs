
If OpenID connect is actively being used by the client application, this should be obvious from the authorization request. The most foolproof way to check is to look for the mandatory `openid` scope.

Even if the login process does not initially appear to be using OpenID Connect, it is still worth checking whether the OAuth service supports it. You can simply try adding the `openid` scope or changing the response type to `id_token` and observing whether this results in an error.

As with basic OAuth, it's also a good idea to take a look at the OAuth provider's documentation to see if there's any useful information about their OpenID Connect support. You may also be able to access the configuration file from the standard endpoint `/.well-known/openid-configuration`.