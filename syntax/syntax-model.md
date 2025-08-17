# Model Syntax 

   # Primary Indexes 🔑
      ```php
         $table->id();                        // Auto-incrementing UNSIGNED BIGINT (primary key)
         $table->bigIncrements('id');         // Big auto-incrementing primary key
         $table->increments('id');            // Auto-incrementing integer (primary key)
         $table->uuid('id')->primary();       // UUID primary key
         $table->foreignId('user_id');        // UNSIGNED BIGINT (used for foreign keys)
         $table->foreignId('user_id')->constrained(); // With foreign key constraint
         $table->unsignedBigInteger('employer_id');
      ```

   # String & Text
      ```php
         $table->string('name');              // VARCHAR(255)
         $table->string('email', 100);        // VARCHAR(100)
         $table->char('code', 6);             // CHAR(6)
         $table->text('description');         // TEXT
         $table->mediumText('bio');           // MEDIUMTEXT
         $table->longText('content');         // LONGTEXT
      ```

   # Numbers 
      ```php
         $table->integer('age');              // INT
         $table->tinyInteger('status');       // TINYINT
         $table->smallInteger('rank');        // SMALLINT
         $table->bigInteger('views');         // BIGINT
         $table->unsignedInteger('points');   // UNSIGNED INT
         $table->decimal('price', 8, 2);      // DECIMAL(8,2)
         $table->float('rating', 5, 2);       // FLOAT(5,2)
         $table->double('amount', 15, 8);     // DOUBLE(15,8)
      ```

   # Date & Time
      ```php
         $table->date('birthday');            // DATE
         $table->datetime('published_at');    // DATETIME
         $table->timestamp('created_at');     // TIMESTAMP
         $table->time('start_time');          // TIME
         $table->year('birth_year');          // YEAR
         $table->timestamps();                // created_at & updated_at
         $table->softDeletes();               // deleted_at (for soft deletes)
      ```

   # Boolean & JSON 
      ```php
         $table->boolean('is_active');        // TINYINT(1)
         $table->json('meta');                // JSON
         $table->jsonb('options');            // JSONB (Postgres only)
      ```

   # Relationships 🔗
      ```php
         $table->foreignId('user_id')->constrained(); // user_id with FK to users.id
         $table->foreignIdFor(App\Models\User::class); // Laravel 7+ model-based FK
      ```

   # Other Useful Types ⚙️
      ```php
         $table->enum('status', ['pending', 'active', 'archived']); // ENUM
         $table->binary('data');             // BLOB
         $table->ipAddress('visitor');       // IP address
         $table->macAddress('device');       // MAC address
         $table->uuid('uuid');               // UUID
      ```