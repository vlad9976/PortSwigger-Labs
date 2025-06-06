
Since mass assignment creates parameters from object fields, you can often identify these hidden parameters by manually examining objects returned by the API.

For example, consider a `PATCH /api/users/` request, which enables users to update their username and email, and includes the following JSON:

`{ "username": "wiener", "email": "wiener@example.com", }`

A concurrent `GET /api/users/123` request returns the following JSON:

`{ "id": 123, "name": "John Doe", "email": "john@example.com", "isAdmin": "false" }`

This may indicate that the hidden `id` and `isAdmin` parameters are bound to the internal user object, alongside the updated username and email parameters.