# Artisan Command 
   - Artisan Commmand Helps development faster

      * Recently we use this command to *run our project*
         ```
            > php artisan serve
         ```
         _Exp_: This will output the link to our project.


      * *Create a view file* called hello
         ```
            > php artisan make:view hello
            //View [resources/view/hello.blade.php] created successfuly
         ```
         _Exp_: This will `create a file helo.blade.php` inside
         of folder 📁view/. 
      *  *Create a view file with sub folder*
         ```
            > php artisan make:view product.index
            //View [resources/view/product/index.blade.php] created successfuly
         ```


      * Generate or *create a controller* 
      ```
         php artisan make:controller productController
         //Controller [app/Http/Controllers/productController] created successfuly
      ```
      _Exp_: This is our controller name productController, this will go in app folder [📁app/Http/Controllers], This will extends automatically the Controlers parent class
     

      * Use components 
         ```
            php artisan make:component Alert 
         ```
         - This will generate new component with this command however this will also generate a
         associated class that we can use to add functionality to the view 


      * We use migrations to create and modify database tables, This will allows yu to include 
      the database schema inside the code base, so its easier to keep it iniside version control
      and much easier for multiple developers to use the same database structure.
      ```
         php artisan make:migration create_product_table
      ```
      * This will create the file in 
         [database/migrations/..._create_product_table.php]
      _N_: The name of the migration file isn't PHP class name so we use the snake case


      * To run this migration we use this command, this will create the database 
         ```   
            php artisan migrate
         ```


      * To use model class we use the  make model artisan command
         ```
            php artisan make:model Product
            // [app\Models\Product.php]
         ```
         This will created inside of app folder
         

      * To view all Route list | This will show all route you created
      ```
         php artisan route:list
      ```
      Some route will show : Can be ignore this
      Route.. Edit by debugbar
      Route.. Edit by ignition 



# How to make a MOdel Migration and Controller at 1 command
   [-] Migration : to create the product table
   [-] Product Model : So that we can interact with that table or migration table you create
   [-] Product Controller : To handle the request and responses.

   - first we neeed to understand the output of this command
   ```
      php artisan make:model --help
   ```
   - Then we can now create the migration, controller, and the model at the sametime
      * all vlass name and table name will be the name you will put in this command
   ```
      php artisan make:model Category -mcr
   ```
   - m: make migration
   - c:  a controller
   - r: to make the controller resource full/ meaning they will have a default method  
   [_i_]: when we run this we will get a MIGRATION, MODEL, CONTROLLER
   - Ready to use 
   - Inside of migration you will just specify some column for table
   - in model we can put some allowed form data to our database
   * Then in Route or web.php we can specify this 
   ```php
      use App\Http\Controllers\CategoryController;

      Route::resource('category' CategoryController::class);

      // The first arguments is 'category' this is a prefix url for all method
   ```
   - This Route is a compatible for our generated controller
   * You can check the route if correct using | **[ php artisan route:list ]**

   - So by generating the model migration and controller at once you can create functionality to create, read, update and delete records very quickly with a minimum of code


