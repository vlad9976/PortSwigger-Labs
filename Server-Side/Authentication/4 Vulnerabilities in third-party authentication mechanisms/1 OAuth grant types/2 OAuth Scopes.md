
For any OAuth grant type, the client application has to specify which data it wants to access and what kind of operations it wants to perform. It does this using the `scope` parameter of the authorization request it sends to the OAuth service.

For basic OAuth, the scopes for which a client application can request access are unique to each OAuth service. As the name of the scope is just an arbitrary text string, the format can vary dramatically between providers. Some even use a full URI as the scope name, similar to a REST API endpoint. For example, when requesting read access to a user's contact list, the scope name might take any of the following forms depending on the OAuth service being used:

`scope=contacts scope=contacts.read scope=contact-list-r scope=https://oauth-authorization-server.com/auth/scopes/user/contacts.readonly`

When OAuth is used for authentication, however, the standardized OpenID Connect scopes are often used instead. For example, the scope `openid profile` will grant the client application read access to a predefined set of basic information about the user, such as their email address, username, and so on