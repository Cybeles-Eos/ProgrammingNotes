# Regular Expression Constraints

   EXAMPLE :
   ```php
      Route::get('/user/{name}', function (string $name) {
      // ...
      })->where('name', '[A-Za-z]+');

      // IS SAME AS ⬇

      Route::get('/user/{id}/{name}', function (string $id, string $name) {
         // ...
      })->whereNumber('id')->whereAlpha('name');
   ```


   ```php
      whereAlphaNumeric('name');  // ==   0-9a-zA-Z
      whereIn('category', ['movie', 'song', 'painting']); 
      
   ```

# Put a cutom method inside of model
   ```php JobModel.php
      <?php
      namespace App/Models;

      use Illiminate\Support\Arr;

      class Job{
         public static function all(): array
         {
            ...
            //Datas
            ...
         }

         //Custom Method
         public static function find($id): array
         {
            return Add::first(static::all(), fn($job) => $job['id'] == $id);
            //static::all() is the all method we have
            // we use static:: because where on the same file.
         }
      }
   ```
   ```php web.php  
      // Function is like our controller.
      Route::get('/jobs/{id}', function (int $id) {
         $job = Job::find($id);

         if(!$job){
            abort(404);
         }

         return view('job', compact('job'));
      });
   ```












