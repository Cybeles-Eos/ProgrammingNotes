# Understanding Code

   - How route uses a dynamic {id}. *Route Parameter*-*Dynamic Parameter*
      ```php web.php
         Route::get('/users/{id}', [UserController::class, 'show']);
      ```
      ```php userController.php
         public function show(String $id){
            return view('users.profile', [
               "user" => Users::findOrFail($id)
            ]);
         }
      ``` 
      ```php profile.php
         <x-layout>
            <h1>{{ $user->name }}</h1>
         </x-layout>
      ```
      _Exp_: 
      * First, we declare the route for our show method. This `route uses a dynamic {id}` parameter to load a specific user's profile based on their ID.
      * In the UserController, the `show method receives the id from the route`. It then uses Users::findOrFail($id) to find the user with that ID, or throw an error if not found.
      * The user data is passed to the users.profile view using the view() function and the associative array ["user" => ...].
      * In the profile.php view, we can access the user's name using {{ $user->name }} because the user object was passed to the view.
      - The part {id} is called a *route parameter — it's "dynamic"* because:
         * It `can change depending on which user you want to view.`
         * It's not a fixed value like /users/5 or /users/10 — instead, Laravel will automatically replace {id} with whatever value is in the URL.














