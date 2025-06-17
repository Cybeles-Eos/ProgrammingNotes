# Redirect user to specified page
   ```php
      return redirect()->route('product.index');
   ```
   * product📁.index📃


# If someone try to access to a method in controller via url address bar
   - we can redirect them to specific url
   - This logic is for form actions attribute. 
   ```php
      Route::post('/products/store', [ProductController::class, 'store'])
         ->name('product.store');
      // Redirect to index page
      Route::get('/products/store', function () {
         return redirect()->route('product.index');
      });
   ```
   _N_: Even the user type in url bar this will not work because we use POST not GET
   POST is working at the backend route, only GET will work in URL bar if someone try to access page.


# If you use Controller the controller have the own index page called index
   - Means its like home page but in Product home page
   thats why we use index
   ```php
      Route::get('/products', [ProductController::class, 'index'])  //.. index method in our productController
      ->name('product.index');           

      Route::get('/products/create', [ProductController::class, 'create'])
      ->name('product.create');
   ```
   - In URL bar this is what index show
   [http://127.0.0.1:8000/products/]
   [http://127.0.0.1:8000/products/index]❌ will show a error or 404page.

