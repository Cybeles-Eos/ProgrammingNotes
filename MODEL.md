# Model in LARAVEL (M)MVC

   - How to connect to our database in LARAVEL 
   _N_: view: is just use to present some data containing just enough logic to do so.

   * In the controller when w erender a view we pass the data to the view that we want
   it to display 

   [o]: To interact with the database we use models 
      * Each database table has acorrespanding model class This is the M in MVC
      _I_: laravel includes an object relational mapper component called Eloquent [https://laravel.com/docs/12.x/eloquent].
      * This mean most of the time you dont need to write a SQl query directly
      Thi Eloquent Includes  method to retrive, insert, update, delete records
   [c]: to use model class we use the  make model artisan command
   ```
      php artisan make:model Product
   ```
      : Or if you dont migrate your database yet, you can use this command
      ```
         php artisan make:model Product --migration
      ```
      * This will create the classs in app\model\Product.php  folder
      * after this you can go to your controlerProduct
      ```php
          <?php
            namespace App\Http\Controllers;

            use Illuminate\Http\Request;
            use App\Models\Product;   // we import the Product model that we create.

            class ProductController extends Controller
            {
               public function index(){
                  $product = Product::all(); // This will get all records, we simply call the static all methods on the product model class
                  return view('products.index');
               }
            }
      ```
      * If you run this you will get a error,
      because by default Eloquent will guess the table name  based on the model class name 
      * you can rename this in php admin, but instead we can configure the model with the actualtble name
      ```php Model/Product.php  
         <?php

         namespace App\Models;

         use Illuminate\Database\Eloquent\Model;

         class Product extends Model
         {
            protected $table = 'Product'; // This will tell to Eloquent this is the table name
         }


      ```
      * If you want to se the data we use var_dump() in PHP but in laravel we use dd()
      ```php
          <?php
            namespace App\Http\Controllers;

            use Illuminate\Http\Request;
            use App\Models\Product;   // we import the Product model that we create.

            class ProductController extends Controller
            {
               public function index(){
                  $product = Product::all(); 
                  dd($product);   // Dump and die
                  return view('products.index');
               }
            }
      ```
      * This will output the records but in clean way

# How to pass a data in view RETIRIVE and DISPLAY DATA
   - In The second argument of view we pass a array this array contain two value
   1 is for variable name the the another is the value what we will pass.
   ```php   productController.php
      <?php 
      namespace App\Http\Controllers;

      use Illuminate\Http\Request;
      use App\Models\Product;   // we import the Product model that we create.

      class ProductController extends Controller
      {
         public function index(){
            $product = Product::all(); // This will get all records, we simply call the static all methods on the product model class
            return view('products.index', [
               "Products" => $product
            ]);
         }
      }
   ```
   - Then in the Products index view we can now use this data
   ```php   products/index.blade.php
      <x-layout>
         <?php foreach($products as $product): ?>
            <h1><?= htmlspecialchars($product->name) ?></h1>
            <h1><?= htmlspecialchars($product->name) ?></h1>
            <h1><?= htmlspecialchars($product->name) ?></h1>
         <?php endforeach; ?>  

         <h1>Products</h1>
      </x-layout>
   ```
   - As we use laravel Blade we can shorten this PHP loop to laravel blade syntax
   ```php  ✅
      @foreach ($products as $product)
        <p>Name:{{ $product->name }}</p>
        <p>Size: {{ $product->size }}</p>
        <p>Description: {{ $product->description }}</p>
        <hr />
      @endforeach
   ```
   - Same output as the top code
   - There are many balde syntax to learn [https://laravel.com/docs/12.x/blade#loops]
   - So now we RETRIVE and DISPLAY data without writng SQl at all.
   - If you want to see the SQL data we use to our frame work we can install a debugger fr laravel
   ```
      composer require barryvdh/laravel-debugbar --dev
   ```
   * This will install a debuger that vissible to page and containing all information about the sql we use or performe in our project.




# How to insert data into the database using laravel
   - In php we use superglobals like $_POST but in laravel 
   to get access to this object we just need to include it as parameter
   to the method
   - We will use the Request method htat auomatic generated in our controller
   ```php  ProductContorller.php
      <?php

      namespace App\Http\Controllers;

      use Illuminate\Http\Request;
      use App\Models\Product;

      class ProductController extends Controller
      {
         public function index(){
            //$product = Product::all();
            //dd($product);
            return view('products.index', [
                  'products' => Product::all() 
            ]);
         }

         public function create(){
            return view('products.create');
         }

         public function store(Request $request){
            $name = $request->input('name'); // or we can use Dynamic properties this will get the same value $request->name same as $request->input('name'); 
            $size = $request->size;
            $description = $request->input('description');
         }
      }
   ```
   - To get the form data to the request object we can use the all method 
   to get all form data 
   ```php
      $requst->all();
   ```
   - Or if you want to get only 1 specific formdata we use input method with an argument 
   ```php
      $request->input('name');
   ```
   [i]: Heres the link of all request method in laravel []

   * To insert new record using Eloquent we create a new model object then assign values to properties
   ```php ProductContorller.php
      ...
      public function store(Request $request){
         $product = new Product;  // This is the name of our table in the MODEL/Product.php

         $product->name = $request->name;
         $product->size = $request->size;
         $product->description = $requst->description;

         $product->save();  // After we set the form data we ready to save this using save method();
         
         return redirect()->route('products.index');   // Then redirect to our index page
      }
   ```
   * Then after this we add this to our web.php
      | Route::post('products/store', [ProductController::class, 'store'])->name('product.store')
   * Then att the action in create.php form
   ```
      <form method="POST" action="{{ route('product.store') }}">....</form>
   ```
   * After this when user click the save button it will `direct you to 404 laravel page expired`
      - This is a *laravel built-in (CSRF)Cross Site request*
      - This mean the framework will generate a unique token for each user session which verifies that the request comes from current user
      - *This is enable by default*. all you need to do is make sure you include field in your form that contains the token value
      * You can do this manually with this code 
      ```php
         <input type='hidden' name="_token" value="{{ csrf_token() }}" />
      ```
      [csrf_protection]
      * Or in simple way you can add or include this csrf blade directive in the form.
      ```php 
         <form>
            @csrf
            ...
         </form>
      ```
   [_i_]: There is a alternaticve or shortcut in `creating object, setting properties, and calling the save` like this code
   ```php ProductContorller.php
      ...
      public function store(Request $request){

         // Manual Assingment
         $product = new Product;  // This is the name of our table in the MODEL/Product.php
         $product->name = $request->name;
         $product->size = $request->size;
         $product->description = $requst->description;

         $product->save();  // After we set the form data we ready to save this using save method();
         
         return redirect()->route('products.index');   // Then redirect to our index page
      }
   ```
   [Mass assignment]
   * Instead we can just call the static cerate method passing in an assosiative array of all the values we want to save.
   ```php
      public function store(Request $request){
         
         Product::create($request->input());  // gets all form data submitted (like name, size, etc.). without passing any arguments
         // create()method is from our import in model Eloquent not from this controller

         return redirect()->route('products.index');

      }
   ```
   * After you set this and run there will be error, saying we need to add `fillable property` to allow mass assignment.
      * mass assignment vulnerability is when unexpected data in the request cahnges a column in the database. 
         Exmp: When editing users there might be a column in the user that required admin permision a
            Malicius user could send a value for this column when creating user record that whould give them a admin access.
   - To prevent that we add a property to the Model that list the columns that are mass assignable 
   ```php Model/Product
      <?php

      namespace App\Models;

      use Illuminate\Database\Eloquent\Model;

      class Product extends Model
      {
         //
         protected $table = 'Product';

         protected $fillable = ['name', 'size', 'description']; //$fillable must correct spelling 


         //public $timestamps = false; // disable all timestamp
      }
   ```
   - $fillable :  it's just a normal PHP property that `Laravel reads for security` when using *create()*, *update()*, or *fill()*.
   - We have the create() method thanks for Eloquent\Moel
   * This `means your model inherits all the powerful methods defined in the Model clas`s — including:
      create()
      update()
      save()
      delete()
      find()
      all()


# validate user inputs 
   - We can validate input by calling the validate() method on the request object with argument of array with value of input to validate.
   ```php
      public function store(Request $request){
         
         $request->validate([
            'name' => 'required|max:100',
            'size' => 'required|decimal:0,2',
            'description' => 'nullable|min:3'
         ]);
         //Or we can use array instead of single string with pipe character(|)
         // $request->validate(
         //   'name' => ['required', 'max:100'],
         //   'size' => ['required', 'decimal:0,2'],
         //   'description' => ['nullable', 'min:3']
         // )

         Product::create($request->input());

         return redirect()->route('products.index');
      }
   ```
   _________________________________________
   [_I_]: Check if there is error using *$errors* variable that contain all validation message
   * In your form at create php.

   ```php
      <x-layout>
         <h1>New Product</h1>
         <a href="{{ route('product.index') }}">Go back</a>

         @if($errors->any())
            <ul>
               @foreach ($errors->all() as $error)
                  <li>{{ $error }}</li>
               @endforeach
            </ul>
         @endif

         <form method="POST" action="{{ route('product.store') }}">
            @csrf

            <label for="name">Name: </label>
            <input type="text" name="name" id="name">
            
            <label for="size">Size: </label>
            <input type="number" step="0.01" name="size" id="size">

            <label for="description">Description: </label>
            <textarea type="text" name="description" id="description"></textarea>
            
            <button type="submit" name="submit">Save</button>
         </form>
      </x-layout>
   ```
   - If there is a error this will show message in the top
   _________________________________________
   [_I_]: How to save old values in input fields even if the page refresh through error.
   * Laravel provides a convinient helper function called *old* that we can use to display these valuse again in input form.
   ```php
      <x-layout>
         <h1>New Product</h1>
         <a href="{{ route('product.index') }}">Go back</a>

         @if($error->any())
            <ul>
               @foreach ($error->all() as $error)
                  <li>{{ $error }}</li>
               @endforeach
            </ul>
         @endif

         <form method="POST" action="{{ route('product.store') }}">
            @csrf

            <label for="name">Name: </label>
            <input type="text" name="name" id="name" values="{{ old('name') }}">
            
            <label for="size">Size: </label>
            <input type="number" step="0.01" name="size" id="size" values="{{ old('size') }}">

            <label for="description">Description: </label>
            <textarea type="text" name="description" id="description">{{ old('description') }}</textarea>
            
            <button type="submit" name="submit">Save</button>
         </form>
      </x-layout>
   ```







