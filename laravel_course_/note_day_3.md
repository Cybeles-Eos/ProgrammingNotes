# Eloquent Model Methods

**[ Retrieving Records ]**
| Method            | Description                               | Returns                |
| ----------------- | ----------------------------------------- | ---------------------- |
| `all()`           | Get all records                           | `Collection` of models |
| `find($id)`       | Find by primary key                       | Model or `null`        |
| `findOrFail($id)` | Find by primary key or throw 404          | Model                  |
| `first()`         | Get the first matching record             | Model or `null`        |
| `firstOrFail()`   | Get the first match or throw 404          | Model                  |
| `get()`           | Execute query and get all matches         | `Collection`           |
| `pluck('column')` | Get array of single column values         | `Collection`           |
| `value('column')` | Get single column value from first result | Single value           |
| `count()`         | Get count of matching records             | Integer                |
| `exists()`        | Check if any matching records exist       | Boolean                |
**[ Retrieving Records - Samples ]**
```php
    $product = Products::get();
    $product = Products::all();
    /*
     *     This will get all active product,
     *     You can use this by looping the $product collection.
    */
```
|
|
|
|
|
**[ Building Queries ]**
| Method                | Description                            |
| --------------------- | -------------------------------------- |
| `where()`             | Add `WHERE` condition                  |
| `orWhere()`           | Add `OR WHERE` condition               |
| `whereIn()`           | `WHERE IN (...)`                       |
| `whereBetween()`      | `WHERE BETWEEN ... AND ...`            |
| `orderBy()`           | Sort the results                       |
| `groupBy()`           | Group results by column                |
| `select()`            | Select specific columns                |
| `with()`              | Eager load relationships               |
| `has()`, `whereHas()` | Filter based on relationship existence |
| `limit()` / `take()`  | Limit number of results                |
| `skip()` / `offset()` | Skip records (for pagination)          |
**[ Building Queries - Samples ]**
```php
    $product = Products::where('name', $request->name)->firstOrFail();
    $product = Products::where('is_active', 1)->first();
    /*
     *     This will get specific active and name product base on,
     *     Queries and use Retriving method to set the value in $product
    */
```



**[ Add helper.php in your Project ]**
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
_____________________________________________________________________________________________________

# Eloquent Explained.
   - In model file when you look in class, you will notice a extends Model 
   from [illuminate\Database\Eloquent\Model].
   ```php
      use Illuminate\Database\Eloquent\Model;

      class Job extends Model {...}
   ```

   - This Means we can now use a built-in Eloquent methods in class method from Model like
   `::all()`, `::find(1)` and etc...
   - If you declare a class inside of that model that extends Model and name it 
   like all, this will cause a error because you extends model and model have the same 
   name you declare.
   ```php
      use App\Model\Job;

      $jobs = Job::all();
   ```

# Eloquent: Table and Model Naming

   1. If you have a model and table then inside of model file this will be the naming.
      solution 1.
      - If you have table post
         📅table --> ["post"]
         📃Eloquent model --> [Post]
         ```php
            class Post extends Model {...}
         ```
         |> This is valid

      - If you have table comments
         📅table --> ["comments"]
         📃Eloquent model --> [Comments]
         ```php
            class Comments extends Model {...}
         ```
         |> This is valid 

   2. Solution 2 you can make a unique table name. by adding a protected property table.
      ```php  ✅
         class Comment extends Model {
            protected $table = "comments" 
            // This tell Eloquent this is the table comments name in DB.
         }
      ```

# Eloquent Model: fillable property
   - The fillable property controls which fields are allowed to be saved or updated in the database. This helps protect your data from unwanted changes.
   ```php
      class Comment extends Model {
         protected $table = "comments" 
         
         protected $fillable = ['title', 'author'];
         // Only 'title' and 'author' can be saved or updated in this model.
         // Other fields will be ignore.
      }
   ```

   