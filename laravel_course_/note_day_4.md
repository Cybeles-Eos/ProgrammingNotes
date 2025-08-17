# All About factory 
   - 📁 database/factories/...
   - Good for dummy data
   - Helps in production of your project.

# UserFactory
   - Use a `model factory` to quickly scaffold example data.
      * A factory is useful in any situation where we need to rapidly `generate test or seed data` — in this case, for a user.
   ```php
      // Sample Of Generated UserFactory By Laravel.
      ...
      public function definition(): array
      {
         return [
               'name' => fake()->name(),
               'email' => fake()->unique()->safeEmail(),
               'email_verified_at' => now(),
               'password' => static::$password ??= Hash::make('password'),
               'remember_token' => Str::random(10),
         ];
      }
      ...
   ```
   * It is used to generate and insert fake data into the project, which is useful for testing functionality or simulating real-world scenarios during development.

# fake() - An API called Faker.
   - fake() is an API that provides many useful methods to generate dummy data.
   Examples: 
      * fake()->name()
      * fake()->firstName()
      * fake()->lastName()
      * etc...
   | Faker methods                          |
   | -------------- | --------------------- |
   | Job title      | `fake()->jobTitle()`  |
   | Paragraph text | `fake()->paragraph()` |
   | Long text      | `fake()->text(200)`   |
   | Random company | `fake()->company()`   |
   | Location       | `fake()->city()`      |


# state in UserFactory
   - This method is used to change column values to specific values you define.
   ```php
      public function unverified(): static
      {
         return $this->state(fn (array $attributes) => [
            'email_verified_at' => null,
         ]);
      }
   ```
   * So all `email_verified_at` values will be set to `null`.
   - You can create more states depending on what you want to change.
   - Example: If you want to change all names to "Steave".
   ```php
      public function name(): static
      {
         return $this->state(fn (array $attributes) => ['name' => 'Steave']);
      }
   ```


# How to generate or run the UserFactory to generate fake data.
   - Open tinker through `php artisan tinker`
   - Then run this in tinker
   ```
      > App\Models\User::factory()->create()
   
      = ...results...
   ```
   *If you want to generate many fake user*
      ```
         > App\Models\User::factory(100)->create()
      
         = ...100 results...
      ```

## Creating new Factory
   - Open terminal
   ```
      // php artisan help make:factory
      // - This will help you if you don't know what to put

      php artisan make:factory JobFactory
   ```
   * Now check the factories folder inside the database you can now see and code in that JobFactory.

   - Put the properties of your table job inside of definition() at JobFactory.
   ```php
      public function definition(): array
      {
         return [
            'title' => fake()->jobTitle(), // fake title from faker api
            'salary' => '$500,000 USD'     // hard coded value 
            
            //'salary' => rand(10000, 50000)  // random number between 10000 to 50000
         ];
      }
   ```
   * Then go to tinker to run the dummy JobFactory
   ```
      > App\Models\Jobs::factory(20)->create()

      = ...20 jobs results...
   ```
   - Laravel is smart enough to know which factory to use based on the model.
      * It's like Laravel automatically knows how to generate the appropriate dummy data using the JobFactory tied to the Jobs model.
   
   🛑 Note:
      - Make sure in your model you include the `HasFactory` to be able to work that properly.
      ```php   inside of Job Model
         use Illuminate\Database\Eloquent\Factories\HasFactory; //✅

         class Job extends Model{

            use HasFactory;    //✅
            ...
         }
      ```




💠 Extra:
   - If you want to add a new column in the Job list that links to the Employer table (to connect Job listings to employers):
   ```php inside of Job_list Migration 
      // Inside Job_list migration

      // You can add a foreign key for employer_id to indicate a relationship

      public function up(): void
      {
         Schema::create('job_listing', function (Blueprint $table) {
            ...
            $table->foreignIdFor(\App\Models\Employer::class);
            // This means each employer's ID will be stored as employer_id
            // employer_id is generated automatically based on the model name + _id
         });
      }
   ```
   - So when generating data using the JobFactory, you now add this:
   ```php
         use App\Models\Employer;

         public function definition(): array
         {
            return [
               'title' => fake()->jobTitle(),
               'employer_id' => Employer::factory(),   //✅
               'salary' => '$500,000 USD'
            ];
         }
   ```
   * Make sure the Employer model has a factory too:
   ```
      php artisan make:model Employer -fm

      // -f for Factory, -m for Migration
   ```
   * INside of EmployerFactory indicate the name in definition method
   ```php
      'name' => fake()->company()
   ```
   - Then run in tinker `php artisan tinker`
   ```
      > App\Models\Job::Factory(300)->create()

      = ...results...
   ```
   - Now the value of employer_id will be base on Employer table ID.
   - If you want to make more than 5 of Employer has the same JOb 
   use `recycle method`.

# Relational Lessons

