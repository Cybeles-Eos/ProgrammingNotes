# Route Group

   - If your route has all same controller we can use controller group to our route
   ```php
      <?php

      use Illuminate\Support\Facades\Route;
      use App\Http\Controllers\ProductController;


      Route::view('/', 'home');
      Route::get('/hello', function () { return view('hello'); });

      Route::controller(ProductController::class)->group(function (){
         Route::get('/products', 'index')->name('product.index');           
         Route::get('/products/create', 'create')->name('product.create');
         Route::post('/products/store', 'store')->name('product.store');
         Route::get('/products/{product}', 'show')->name('product.show');
         Route::get('/products/{product}/edit', 'edit')->name('product.edit');
         Route::patch('/products/{product}', 'update')->name('product.update');
         Route::delete('/products/{product}', 'delete')->name('product.delete');
      });
   ```
   - Another tips to short this, as you can see they all share the same products prefix in the first segmentof the root, we can specify this with *prefix method*.
   ```php
      <?php

      use Illuminate\Support\Facades\Route;
      use App\Http\Controllers\ProductController;


      Route::view('/', 'home');
      Route::get('/hello', function () { return view('hello'); });

      Route::controller(ProductController::class)
         ->prefix('/products')
         ->group(function (){
         Route::get('/', 'index')->name('product.index');           
         Route::get('/create', 'create')->name('product.create');
         Route::post('/store', 'store')->name('product.store');
         Route::get('/{product}', 'show')->name('product.show');
         Route::get('/{product}/edit', 'edit')->name('product.edit');
         Route::patch('/{product}', 'update')->name('product.update');
         Route::delete('/{product}', 'delete')->name('product.delete');
      });
   ```
   - same as name there are all same product at the first name
   we can use a name method for this 
      ```php
      <?php

      use Illuminate\Support\Facades\Route;
      use App\Http\Controllers\ProductController;


      Route::view('/', 'home');
      Route::get('/hello', function () { return view('hello'); });

      Route::controller(ProductController::class)
         ->prefix('/products')
         ->name('product.')
         ->group(function (){
         Route::get('/', 'index')->name('index');           
         Route::get('/create', 'create')->name('create');
         Route::post('/store', 'store')->name('store');
         Route::get('/{product}', 'show')->name('show');
         Route::get('/{product}/edit', 'edit')->name('edit');
         Route::patch('/{product}', 'update')->name('update');
         Route::delete('/{product}', 'delete')->name('delete');
      });
   ```








































