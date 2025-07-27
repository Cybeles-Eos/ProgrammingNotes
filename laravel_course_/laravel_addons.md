# Laravel Usefull Code

# **[ Add helper.php in your Project ]**
- Create helper at app/helper.php
- Write your function inside it
- Autoload the helper in composer.json
```json
 "autoload": {
    //...
    "files": [
        "app/helpers.php"
    ]
}
```
- Run Composer dump-autoload
```
    composer dump-autoload --no-scripts
```
- Now You Can Use the Function Anywhere
```php
public function index()
{
    $siteName = getSiteName();
    return view('home', compact('siteName'));
}
```

#

