# Relational Lessons

   # To Make a primary ID in table with existing primary Id in relational 

      ```php
         $table->unsignedBigInteger('employer_id');

         //or 

         $table->foreignIdFor('\App\Models\Employee::class');
      ```


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
      public function employee()
      {
         return $this->belongsTo(Employee::class);
      }
   ```
   ✔ Put this on the model that has the foreign key column.
      - Like when your Job table has a employee_id foreign key.

   
   

























