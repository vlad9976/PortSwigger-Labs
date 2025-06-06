
In some cases, sensitive functionality is concealed by giving it a less predictable URL. This is an example of so-called "security by obscurity". However, hiding sensitive functionality does not provide effective access control because users might discover the obfuscated URL in a number of ways.

Imagine an application that hosts administrative functions at the following URL:

`https://insecure-website.com/administrator-panel-yb556`

This might not be directly guessable by an attacker. However, the application might still leak the URL to users. The URL might be disclosed in JavaScript that constructs the user interface based on the user's role:

`<script> var isAdmin = false; if (isAdmin) { ... var adminPanelTag = document.createElement('a'); adminPanelTag.setAttribute('https://insecure-website.com/administrator-panel-yb556'); adminPanelTag.innerText = 'Admin panel'; ... } </script>`

This script adds a link to the user's UI if they are an admin user. However, the script containing the URL is visible to all users regardless of their role.