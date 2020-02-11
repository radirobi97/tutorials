# Basic form handling

```html
<form action="form.php" method="POST">
  <input type="text" placeholder="Enter Username" name="username">
  <br>
  <input type="submit" name="submit">
</form>
```
**Action** defines where to send the information for processing. <br/>
**Method** defines how to get the information. <br/>
Based on **Name** we can find the proper information between superglobals.

```php
if(isset($_POST["submit"])){
  echo "information can be processed";
}
```
`isset()` checks wether a given variable is set or not.<br/>
`unset()` variable can be unset.
