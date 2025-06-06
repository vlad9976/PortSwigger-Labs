
To test whether you can modify the enumerated `isAdmin` parameter value, add it to the `PATCH` request:

`{ "username": "wiener", "email": "wiener@example.com", "isAdmin": false, }`

In addition, send a `PATCH` request with an invalid `isAdmin` parameter value:

`{ "username": "wiener", "email": "wiener@example.com", "isAdmin": "foo", }`

If the application behaves differently, this may suggest that the invalid value impacts the query logic, but the valid value doesn't. This may indicate that the parameter can be successfully updated by the user.

You can then send a `PATCH` request with the `isAdmin` parameter value set to `true`, to try and exploit the vulnerability:

`{ "username": "wiener", "email": "wiener@example.com", "isAdmin": true, }`

If the `isAdmin` value in the request is bound to the user object without adequate validation and sanitization, the user `wiener` may be incorrectly granted admin privileges. To determine whether this is the case, browse the application as `wiener` to see whether you can access admin functionality.