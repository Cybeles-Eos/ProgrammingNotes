# Relational Lessons

   # To Make a primary ID in table with existing primary Id in relational 

      ```php
         $table->unsignedBigInteger('employer_id');

         //or 

         $table->foreignIdFor(\App\Models\Employee::class);
      ```


   ### Just use hasMany and belongsTo in the two models, and you’ll be able to access each other’s values.
   
   # hasMany
   - *Use when*    : The other table holds the foreign key.
   - *Description* : One model → many related rows in another table.
   - *Example*     : One Employee has many Jobs (jobs.employee_id).
   
   ```php
      public function jobs()
      {
         return $this->hasMany(Job::class);
      }

      // php artisan tinker
      // > $employee = App\Models\Employee::first()
      // > ...results
      // 
      // > $employee->jobs
      // > ...employee job result
   ```

   # belongsTo
   - *Use when*    : This table has the foreign key.
   - *Description* : Many models point back to one parent.
   - *Example*     : A Job belongs to one Employee (jobs.employee_id).
   ```php
      public function employer()
      {
         return $this->belongsTo(Employee::class);
      }
   ```
   ✔ Put this on the model that has the foreign key column.
      - Like when your Job table has a employee_id foreign key.

   # Create Dummy Values
   ```
      > App\Models\Joblist::factory(20)->create()
   ```
   
   # Access Joblists Employee
   ```
      > $job = App\Models\Jobs::first();
      
      > $job->employer;
         = App\Models\Employer{#15828
            id: 1,
            name: "Dawn",
            created_at: "2024-03-25 14:52:53",
            update_at: "2024-03-25 14:52:53",
         }

      > $job->employer->name
         = "Dawn"
   ```

   # Access Employee Job
   ```
      $employer = App\Models\Employer::first()

      $employer->job
         Illuminate\Database\Eloquent\Collection = This Return a collection of value or array
         [{
            id: 1,
            title: "Job Title",
         }]

      $employer->job[0]->title
         = "Job Title"
   ```

























