# Controller 
   - We pu application code in controllers 
   - Controllers are just classes that `contain method`
   that `run when we assign them to a route`
   - This conttroller can containe a method
   for example a user controller that handle all
   request related in showing and editing user records
   * This is the C in MVC

# First we create a view file with sub folder
   |   php artisan make:view product.index

   * We use . to specify the folder product
   then we create our controller
   |   php artisan make:controller productControllers
   
   * Then inside of our product controller we write our view that pointing to our
   view product.index
      ```php
         <?php

         namespace App\Http\Controllers;

         use Illuminate\Http\Request;

         class pruductController extends Controller
         {
            public function index(){
               return view('products.index');
            }
         }
      ```
      - The 'products.index' is the path or our index the . means
      direct to index.blade.php

   * Then inside of route folder in web.php file
   we import the controller class using namespace with
   use
      ```php  web.php
         ...
         use App\Http\Controllers\productController;

         ...
      ```
   * Then afer this we use Route get method
      ```php
         Route::get('/product', [productController::class, 'index ']);
      ```
      - In this instead of callback we pass an array with the
      name of the class which will use the class constant followed by 
      the name of the method in this case index, because the name of our method
      inside our productController is index()




