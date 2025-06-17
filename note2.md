# The Route in Laravel
   - Route class in laravel is know as `Facades`

   ```php   📁routes/web.php 
      <?php

      use Illuminate\Support\Facades\Route;

      Route::get('/', function () { return view('welcome'); });

   ```
   * The [::] is called `Scope Resolution` used to access methods, constants, or static properties from a class without creating an object (*if they are declared as static*), or to refer to class constants or parent methods.

   * 1️⃣ The first argument in this code is the path '/' that will be match 
   to the URL path
   * so the '/' path is like a main home page.
   * 2️⃣ The second argument is a callback function that will be executed 
   if it matches the url path, so the view('welcome') will be running when
   we go to the home page.

   _M_: Here is my example using this Facades Route class
   ```php
      Route::get('/about', function(){ echo "hello this is <b>about page</b>"; });
   ```
   * So when you type in url addres 127.0.0.1:8000/about this will go in about page then
   the result will be the callback function that we echo.
   _N_: Although we can output literal content directlyfrom inside a root like this 
   this isnt how we do it either.❌


   - In laravel content that we send to the browser which is typically HTML 
   stored in [view] template. This is the V in MVC
   - views are used to separate the presentation code from application code
  
  # Where to find the welcome file inside the view
   ```php
      Route::get('/', function () { return view('welcome'); });
   ```
   - In this example the welcome inside of view is a php file located 
   at [📁resources/] in laravel this contain css,js,views folder
   - Inside of views we can now see the welcome.blade.php.
   - Inside of welcome.blade.php we can see now the content 
   that where seeing in the web page.
   - The real name of welcom is welcome.blade.php
   but we can call only the welcome.


   # When your Route only need to return view
      - You can use view method
      ```php
         Route::view('/', 'home');
      ```

   # How to add a robust way to make a href link that go to product instead of hardcoded href='/product'
      ```php  web.php
         Route::get('/product', [ProductController::class, 'index']);
            ->name('product.index'); // This need be a unique name
                                     // We specify a name to our product route
      ```   
      ```php  home.php
         <a href="<?= route('peoduct.index'); ?>">Product</a>
      ```
      - Using shorthand echo we use view to direst it in the product page
      - The advantage of doing this is that if we change url but keep the name the same 
      we dont need to change any of the links as theyll be genereated automatically.
         * like if we change The /product to /pls, the link in href a tag will 
         go to this /pls because we use this route name to href link so it automatic
         change 
      * Instead of short echo we can use a blade syntax
      we will use a two set of curly braces to output the value
      ```php
         <a href="{{ route('peoduct.index'); }}">Product</a> ✅
      ```
      - We can use this blade syntax instead of pure php
      _N_: Using this Blade's {{ }} echo statements are `automatically sent through PHP's htmlspecialchars` function to prevent XSS attacks.
         - If you dont want to use htmlspecialchars use escaped curly
            Hello, {!! $name !!}
         ⚠️ But this is not recommended.

         

   # Instead of repeating our self in every template | using blade we can create a common layout 
      - we can do this using *components or template iheritance*.
      - Lets use components, this are `reusable self-contained chunks of interface code`.
      * we use a annonymous layout component for view 
         ```php
            php artisan make:component layout --view
         ```
         - This will create a component file 
         - The --view flag tells Laravel to `generate a Blade component that uses only a view`, not a class.

      * then after you create this layot.blade.php
      we can now go to other blade.php file to copy
      * To copy a template first you create a x- follow with component name 
      ```php product.blade.php
         <x-layout>
            <h1>Products</h1>
         </x-layout>
      ```
      * whatever inside the x-layout will be pass in layout.blade.php
      * Then we use this component in our layout file
      by using $slot
      ```php  blade.blade.php
         {{ $slot }}
      ```
      {{ $slot }} is a special variable that `represents the content you pass into the component`.
      * now we can style this in our layout file then it will save this style in
      product.blade or in other page with x-layout wrap.
