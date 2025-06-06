
If you're able to modify the query string, you can then attempt to add a second valid parameter to the server-side request.

#### Related pages

For information on how to identify parameters that you can inject into the query string, see the Finding hidden parameters section.

For example, if you've identified the `email` parameter, you could add it to the query string as follows:

`GET /userSearch?name=peter%26email=foo&back=/home`

This results in the following server-side request to the internal API:

`GET /users/search?name=peter&email=foo&publicProfile=true`

Review the response for clues about how the additional parameter is parsed.