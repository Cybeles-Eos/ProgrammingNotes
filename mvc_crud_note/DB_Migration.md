# DATA BASE MIGRATION

   * We use migrations to create and modify database tables, This will allows yu to include 
      the database schema inside the code base, so its easier to keep it iniside version control
      and much easier for multiple developers to use the same database structure.
      ```
         php artisan make:migrations create_product_table
      ```
      * This will create the file in 
         [database/migrations/..._create_product_table.php]
      _N_: The name of the migration file isn't PHP class name so we use the snake case
   
   * Inside of class we have a class that contains two methods
      1. The up() method is use to modify the database `create table`, `add columns` and `index to existing table` and so on
      2. The down() method should do the oposite, like removeing a column that added, deleting a table and so on.

   - Inside of this method the Laravel schema Builder is used to create and modify tables 
   There are methods to create A table add columns rename table and so on.

   ```php
      /**
       * Run the migrations.
       */
      public function up(): void
      {        
         // Our table is called 'product' base on the name we gave in migration file
         // Follow by callback function whose argument is an object
         //  that represent tha $table by default.
         Schema::create('product', function (Blueprint $table) {
               $table->id(); // We Use an id() method to create id with autoincrement integer
               $table->timestamps(); // timestamp() method adds timestamp columns called created_at and updated_at thi will add in db.
                                    // This is an optional this can be delete if you like
               $table->string('name', length: 100); // This will add a column in DB with a varchar and defaulth length is 255, but you can define the length with length: argument Eg.('name', length:100)
               $table->text('description')->nullable(); // This will be a TEXT type, default columns dont accept null value you can specify this by adding a call to the neunable method ('description')->nullable()
               $table->decimal('size', total: 8, places: 2)->default(0); // It can store values like: 123456.78, 45.00, 0.01 or 0 by default.
         });
      }

      /**
       * Reverse the migrations.
       */
      public function down(): void
      {
         Schema::dropIfExists('product'); // Generator automatic do this code for us to make our upmethod reverse
      }

   ```
   * by default the migration folder database isn't emptythey already have some migration in this folder that will
   create some default table that are used by the framework for typical opration cache jobs, user, you can just leave these in. 

   * To run this migration we use this will create the database 
      ```   
         php artisan migrate
      ```

   * If you want to undo the migration you can roll back the command 
      ```   
         php artisan migrate:rollback
      ```
      _i_: This will remove all database in DB
      but not deleted in our project, to publish it again use migrate again































