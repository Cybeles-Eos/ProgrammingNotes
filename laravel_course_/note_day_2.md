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