# View Individual Records
   - To view each product user 
   * First we create a show view page
   ```
      php artisan make:view products.show
   ```
   * Then in the route we will use a curly bracket to specify a value 
   [web.php__ROUTE]
   ```php 
      Route::get('/product/{$id}', [ProductController::class, 'show'])
         ->name('product.show')
         ->whereNumber('id'); //✅
         // This whereNumber is same as where('id', '[0-9]+') means only numeric value will allow, this is called required parameter.
   ```
   - required parameter [https://laravel.com/docs/12.x/routing#route-parameters]
   * Then after this we go in our controller
   [ProductController.php__CONTROLLER]
   ```php
      <?php

      namespace App\Http\Controllers;
      use Illuminate\Http\Request;
      use App\Models\Product; //[MODEL\Product]

      class ProductController extends Controller
      {   
         
         ....

         public function show(string $id){  //string $id is our parameter that send to Route

            // We use query builder to perform a query, 
            // This will perform the query on the product table.
            $product = Product::select('*')// in default column are all selected with asterisk(*)
                              ->where('id', $id)
                              ->get();

            $product = Product::where('id', $id) // So we can remove select(*)
                              ->get();

            // Instead of building query like this, the model give us a method called find()
            //  which retrieves a model using its primary key, means in DB id is set primary key
            $product = Product::find($id); //✅

            //Now lets pass this to our view to use this data there in show php
            return view('product.show', [
               'product' => $product
            ]);
            // oR in shortcut to simplified this, using compact method the variable will be same as $product 
            // compact() will return an array where the name we pass in the index and the value is the variable with that name.
            return view('product', compact('product')); //✅
            //compact('product') = ['product' => $product]
         }
      }
   ```
   * Then in view show lets display this product value there
   ```php

      <x-layout>
         <h1>Name: {{ $product->name }}</h1>
         <p>Size: {{ $product->size }}</p>
         <p>description: {{ $product->description }}</p>
      </x-layout>

   ```
   * The if we try to write in URL bar the product id that does not exist will get error saying attempt on reading null
      - this is because if the find method dint fond the find method returns null
      - we can check this in our controller.
      ```php
         public function show(string $id){  
            $product = Product::find($id);
            if($product === null){
               abort(404); // throw an HTTP exception for the given code 
                           // Then the framework will catch this and display the 404 page
               // return redirect()->route('custom.404page'); // or your custom 404 page
            }

            return view('product', compact('product')); 
         }
      ```
      * But instead checking this in if we can achive the same result by defining this.
      ```php
         public function show(string $id){  
            $product = Product::findOrFail($id); 
            // This is the same as the code above
            // cons: you cannot add custom 404 page


            return view('product', compact('product')); 
         }
      ```
   # Another way to reduce this all code even further, using *ROUTE MODEL BLINDING*
      - At this moment the router is passing the ID from into the root parameter to this method as an argument an were looking up to the record base on the product id 
      - So instead of string argument for the ID well specify an argument of the product model class class called Product
      ```php
         public function show(Product $product){  
            $product = Product::findOrFail($id); // Then we don't need to use this either
            return view('product', compact($product)); 
         }
      ```
      * Then in Route config file instead of id parameter we specify this as product
      ```php web.php
                              // This will automatically look up the records based on thje ID value in this segment
                              // This automatically dosen't match the boot if there is a non-numeric value in that segment so we dont need to use the whereNumber() constraint either, this will work at the same way.
         Route::get('/product/{product}', [ProductController::class, 'show'])
            ->name('product.show');
      ```
      ```php
         public function show(Product $product){  
            //$product = Product::findOrFail($id); 
            // Then we don't need to use this either because product automatically show a 404 page if there is not found in the database
            return view('product', compact('product')); 
            
         }
      ```
      _OVER ALL:_
      ```php  web.php
         Route::get('/product/{product}', [ProductController::class, 'show'])
            ->name('product.show');
      ```
      ```php  productController.php
         public function show(Product $product){  
            return view('product.show', compact('product')); 
         }
      ```
      * So we can now do this in our index php product
      ```php
         <a href="{{ route('product.show', $product->id) }}">{{ $product->name }}</a>
      ```
      - we use the route helper passing the name of the root.
      - the second argument is for the product id to show, This will insert the id of product into the segment that contains the root argument for the product.


# Edit a existing Records 
   - Add functionalities to the existing records.
   - First make component for error and form
      | php artisan make:component error --view
      | php artisan make:component product.form
   ```php error component
      @if($errors->any())
         <ul>
               @foreach($errors->all() as $error)
                  <li>{{ $error }}</li>
               @endforeach
         </ul>
      @endif
   ```
   ```php product.form component
      @csrf   <!-- Cross Site request: enable by default -->

      <label for="name">Name: </label>
      <input type="text" name="name" id="name" placeholder="e. g. Juan" value="{{ old('name') }}">

      <label for="size">Size: </label>
      <input type="number" name="size" id="size" step="0.01" value="{{ old('size') }}">

      <label for="size">Description: </label>
      <textarea name="description" id="description">{{ old('description') }}</textarea>

      <button type="save" name="save">Save</button>
   ```
   - Make a edit view page.
      | php artisan make:view product.edit
   - Inside of edit.php
   ```php
      <x-layout>
         <h1>Product Edit</h1>

         <x-error />  // Error Component

         <form action="">

            <x-product.form /> // Form Component

         </form>
      </x-layout>
   ```
   - Then Specified in Routes.
   ```php  web.php
      Route::get('/product/{product}/edit', [ProductController::class, 'edit'])
         ->name('product.edit');
   ```
   - Then inside of productController
   ```php  ProductController.php
      ....
      public function edit(Product $product){
        return view('product.edit', compact('product'));
      }
   ```
   - then we set a tag to edit inside of show .php
   ```php  show.php
      <h1>{{ $product->name }}</h1>
      <p>{{ $product->size }}</p>
      <p>{{ $product->description }}</p>
      <a href="{{ route('product.edit', $product->id) }}">Edit</a>
   ```

   [_F_]: Inside of our form component we use the old method 
      - The first argument in this old method() is to redisplay previously entered value
      - THe second argument, lets us assign a default value to return if there isn't any previous value, so we will pass the name property of the product object
      ```php
         @csrf   <!-- Cross Site request: enable by default -->

         <label for="name">Name: </label>
         <input type="text" name="name" id="name" placeholder="e. g. Juan" value="{{ old('name', $product->name) }}">

         <label for="size">Size: </label>
         <input type="number" name="size" id="size" step="0.01" value="{{ old('size', $product->size) }}">

         <label for="size">Description: </label>
         <textarea name="description" id="description">{{ old('description', $product->description) }}</textarea>

         <button type="save" name="save">Save</button>
      ``` 
      - This will get us error cuz variable aren't automatically pass to the component 
      - To pass data to a component we use  HTML attribute strings
      - To pass a PHP variable however we need to prefix this with a semi colon(:)
      ```php edit.php
         <x-layout>
            <h1>Product Edit</h1>

            <x-error />  // Error Component

            <form action="">

               <x-product.form :product='$product'/> // Form Component

            </form>
         </x-layout>
      
      ```
      - Now we will get error in our create.php
      - To prevent this we will use the ?? or known as null coalescing value
      ```php  form.php
         <input type="text" name="name" id="name" placeholder="e. g. Juan" value="{{ old('name', $product->name ?? '') }}">
         <input type="number" name="size" id="size" step="0.01" value="{{ old('size', $product->size ?? '') }}">
         <textarea name="description" id="description">{{ old('description', $product->description ?? '') }}</textarea>
      ```
      - This will check if the component has receive a product from the parent.
      * After this we need to create new method to our controller for our edit method 
      ```php
         public function update(Request $request, Product $product){
            
            return view('product.update');
         }
      ```
      - Then add our path to the route 
      ```php
         Route::patch('/product/{product}', [ProductController::class, 'update'])
            ->name('product.update');  //we also assign a suitable name.
      ```
      - Then back in the edit view
      ```php
           <form method='post' action="{{ route('product.update', $product) }}">
            @method('PATCH'); // this will tell the form is path 
                              // patch is not supported in html so we use @method
                              // This is a hidden field to spoof the method
            <x-product.form :product='$product'/> // Form Component
         </form>
      ``` 
      - Method Field
      - Since HTML forms can't make PUT, PATCH, or DELETE requests, you will need to add a hidden _method field to spoof these HTTP verbs. The @method Blade directive can create this field for you:

      * In store method the first code were doing is to validate the data
      ```php  productController.php
         $request->validate([
            "name" => 'required|max:100',
            "size" => 'required|decimal:0,2',
            "description" => 'nullable|min:3'
         ]);
      ```
      - In this we want to do the same thing in update method but without repeating this code 
      - To do it we can create a form request class to encapsulate the validation rules
      we can do this by artisan command
      ``` //✅
         php artisan make:request SaveProductRequest
      ```
      - This will Created at[App\Http\Request\SaveProductRequest]
      - Inside of this SaveProductRequest there is a 2 method generated by default 
      the first one is authorized method use for authenticating users which were not
      going to use in the product so change the value to true
      ```php
         public function authorize(): bool{
            return true; // by default its false
         }
      ```
      - The second method is the rules method, this returns the validation rule that apply to this request
      - so lets copy the content of validate method
      ```php
         "name" => 'required|max:100',
         "size" => 'required|decimal:0,2',
         "description" => 'nullable|min:3'
      ```
      - then paste this  in the rules() method
      ```php
         return[
            "name" => 'required|max:100',
            "size" => 'required|decimal:0,2',
            "description" => 'nullable|min:3'
         ];
      ```
      - Then add this in the top of our controller using use state
      ```php
         <?php
         namespace App\Http\Controllers;
         use Illuminate\Http\Request;
         use App\Models\Product;
                  
         use App\Http\Request\SaveProductRequest; //✅
         // Then change the store method to use this class same in update method
         class ProductController extends Controller
         {   
            
            ...

            public function store(SaveProductRequest $request){
               $table = new Product;  //call our database table in Models/Product & store in in $table
               //$request->validate([
               //      "name" => 'required|max:100',
               //      "size" => 'required|decimal:0,2',
               //      "description" => 'nullable|min:3'
               //]);  we can remove this now
               ...
              // Product::create($request->input()); //Instead of this we will use the validate 
               Product::create($request->validate());
            }
            ...
            public function update(SaveProductRequest $request, Product $product){
               
               $product->update($request->validated()); // update using model object
               // Note were calling the method on the object and not on the class like we did on the store method.
               return redirect()->route('product.show', $product);
            }
         }
      ```
      - Doing this well validate the request data automatically using the rules we added to that class before the controller method is called.

# How to delete a record
   - First we make a delete method in our productController
   ```php
      public function delete(Pruduct $ruduct){
         $ruduct->delete();
         return redirect()->route('product.index'); // If delete success return in index php
      }
   ```
   - We Create the route for this method
   ```php web.php
      Route::delete('/product/{product}', [ProductController::class, 'delete'])
         ->name('product.delete');
   ```
   - In show page we will put the delete form button
   ```php  show.php
      <x-layout>
         <h1>{{ $product->name }}</h1>
         <p>{{ $product->size }}</p>
         <p>{{ $product->description }}</p>
         <a href="{{ route('product.edit', $product->id) }}">Edit</a>

         <form method="post" action="{{ route('product.delete', $product) }}">
            @csrf
            @method('delete')
            <button>Delete</button>
         </form>
      </x-layout>
   ```



# Flash Message 
   [Form_Data]
   - Flash message is like  alert This know as Flash Data
   - This is automatically remove after the next request

   - We can display this in continent way using blade syntax
   ```php ProductController
      public function store(SaveProductRequest $request){
        $prod = Product::create($request->validated()); 
        return redirect()->route('product.show', $prod)
            ->with('status', "Product Saved"); //✅
      }

      public function update(SaveProductRequest $request, Product $product){
         $product->update($request->validated()); 
         return redirect()->route('product.show', $product)
            ->with('status', "Product updated");//✅
      }

      public function delete(Product $product){
         $product->delete();

         return redirect()->route('product.index')
            ->with('status', "Product deleted"); //✅
      }
   ```
   then in the layout component where we wrap our all php with layout
   ```php
      <body>
         @if(session('status'))
            <div>{{ session('status') }}</div> <!--✅-->
         @endif
         
         {{ $slot }}
      </body>
   ```


# Pagination  | To view only 3 data or more
   - This will only show 3 or more data base in the specify number.
   ```php  productController
      public function index(){
         //$product = Product::all();
         //dd($product);
         return view('products.index', [
            'products' => Product::paginate(3);
            // So now only 3 items will show in index page.
         ]);
      }
   ```
   - Inside of index we will add a navigator for our paginate
   ```php  index php
      <a href="{{ route('product.create') }}">Create</a>
      <hr />
      <br><br>
      @foreach ($products as $product)
         <a style="font-size: 1.4rem" href="{{ route('product.show', $product->id) }}">{{ $product->name }}</a>
         <hr />
      @endforeach
      {{ $products->links() }}
   ```
   - We will use the links() method

# How to add some custome pagination 
   - Laravel has a several templates for pagination with different CSS
   [https://laravel.com/docs/12.x/pagination#customizing-the-pagination-view]
   - We will copy custom pagination using this artisan command
   ```
      php artisan vendor:publish --tag=laravel-pagination
   ```
   - This will install all pagination style in vendor folder
   - To use this you can include this in your link() method
   ```php  index php
      <a href="{{ route('product.create') }}">Create</a>
      <hr />
      <br><br>
      @foreach ($products as $product)
         <a style="font-size: 1.4rem" href="{{ route('product.show', $product->id) }}">{{ $product->name }}</a>
         <hr />
      @endforeach
      {{ $products->links('vendor/pagination/simple-default') }} //✅
   ``` 
   - You can style this inside the simple-default.blade.php 
   * We can sort the value of database in index page by doing this
   ```php
      public function index(){
         return view('products.index', [
            'products' => Product::orderBy('created_at')->paginate(3);
         ]);
      }
   ```
   * This will order the prodcut by id.


