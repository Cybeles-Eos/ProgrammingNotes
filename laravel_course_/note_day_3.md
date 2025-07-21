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

   