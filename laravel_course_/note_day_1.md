
            
# .............................. #
#      :: Day 1 - Notes ::       #
# .............................. #


# Route
   - Laravel routes accept a `URI` and a `closure`, which will be called when 
   a GET request matches the URI.
   [web.php]
      ```php  
         Route::get('/', function (){ return view('website.home') });
         // Will Return the home page.

         Route::get('/greeting', function(){ return "Hello World"; });
         // Will return the hello world      
      ```
   _Explanation_: Route get if the url link is equal to / then
  |               return the view home page inside of website folder.


# Layout File
   - Layout File Or Folder is like a `main structure` that connect your all 
   page and components together in just 1 file.
   - We can make more layout just define folder that name layout inside you can create 
   many as you want.
   [component.layout.frontend]
      ```php  frontend.blade.php 
         ...

            <body>
               {{$slot}}
            </body>

         ...
      ```
      ```php  home.blade.php
         <x-frontend>
            <h1>This is Home Page</h1>
         </x-frontend> 
      ```
   _Explanation_: Inside of folder component > layout > frontend.blade.php is our
   |               main layout or structure in our page the $slot is a special variable that let us put the 
   |               content whenever we wrap some elements inside of <x-frontend></x-frontend> component layout 


# Named Slot
   - Instead of using a variable name direct in x-frontend, we can use name slot
   [home]
      ```php
         <x-frontend>
            <x-slot:heading>
               Home Page
            <x-slot:heading>

            <h1>This is Home Page</h1>
         </x-frontend> 
      ```
      ```php  frontend.blade.php 
         ...

            <body>
               <h1>{{$heading}}</h1>
               <div class="app-content">
                  {{$slot}}
               </div>
               
            </body>
         ...
      ```
   _Explanation_: This will print the Home page if we declare the $heading in our main layout. 
   **What we USE**
     - [$slot]: A special variable that holds the content passed between a component's 
     opening and closing tags.

# Request Helper function
   - request is a helper in Laravel that `lets you get data from the current page visit`, like `form inputs`, `the URL`, or the method used (GET, POST, etc.).
   - For example we can use this for conditional styling
   ```php
      <a href="/" class="{{ request()->is('/') ? 'active' : ''}} text-sm">Home</a>
      <a href="/about" class="{{ request()->is('about') ? 'active' : ''}} text-sm">About</a>
      <a href="/contact" class="{{ request()->is('contact') ? 'active' : ''}} text-sm">Contact</a>
   ```
   _Explanation_: First we use `request function to grab information` about the `current request` then 
   second we use `is() method` this allow us to pass a `Rego expression a pattern` to 
   determine *if the current page matches that pattern*.

   * Request Function Common Usages.
   [request()]
   ```php
      request()->method();           // GET, POST, etc.
      request()->path();             // Returns the URI path, e.g. "greeting"
      request()->is('admin/*');      // Check if path matches a pattern
      request()->input('email');     // Get specific input field
      request()->has('name');        // Check if 'name' exists in input
   ```

   * Request in controller
      ```php  pageController
         public function store(Request $request){
            $name = $request->input('name');  //
         }
      ```
   **What we USE**
      - [request()] : Grab information in current request.
      - [is()] : pass a Rego expression a pattern.


# Props and attributes in component | 
   - Props pass data into a component using custom attributes.
      * Use Blade directive: @props(['name', 'age'])
      * Usage In parent: <x-user-card name="Kishibe" age="30" />
      * In component: {{ $name }}, {{ $age }}
   - We use a blade directive(@) to define props.
   [components.frontend]
   ```php
      <ul>         // If request is match / then the value of active props is true
         <x-nav-link href="/" :active="request()->is('/'))">Home</x-nav-link>
         <x-nav-link href="/about" :active="request()->is('about'))">About</x-nav-link>
         <x-nav-link href="/contact" :active="request()->is('contact'))">Contact</x-nav-link>
      </ul>
   ```   
   [components.nav-link]
   ```php
      @props(['active' => false]); // We specify false as a default value to our prop.

      // Will put all attributes that we pass in component tag.
      // We put a href attribute so this will go to our a tag.
      <a {{$attributes}} class="{{ $active ? 'active-style' : 'notActive-style'}} text-sm">
         {{$slot}}
      </a> // Home, About, Contact
   ```
   _Explanation_: [47:00]
      1. We use component to not repeat a condition and make its cleaner.
      2. We use prop to indicates wether this nav link should be marked as the active
         * A `prop will tel the html that this is a not attribute` instead this is a prop
         * If we inspect the browser then hover our nav the `:active="" prop will not show in inspect`
         because `we declare it as a prop inside of our main nav component`.
         * We *Use the : right after the active(:active)* to tell the browser that what we will pass 
         is *not just a regular string instead a expression*.
         - You will notice if you put : next to your prop Exp(:active"true") the true will change 
         color in yellow or orange not in green indicating that this value is a boolean.
      3. We use the {{$attributes}} inside of <a> tag in our component nav-link, This is like a $slot
      a special variable, The `$attributes variable will spit out all attribute` that we might pass in out on our component tag Exp:<x-nav-link href="/contact"> This mean in our <a> tag in component this will automatically have a href attribute with url link.
   
   **What we USE**
      - [:] Binding indicator : It tells Blade this is a PHP expression, not just a plain string.
      - [@props()] Props : Allows us to define and use attributes passed into the component as variables.
        |                  This props attribute is not visible in inspect mode.
      - [{{$attributes}}] : Represents HTML attributes passed to a component that aren't defined as props.
        |                   Commonly used to allow passing class, id, style, etc.
        |                   Will pass all attribute in specific tag without using props or variable.


# @php
   - By using this blade directive it allows as to write raw PHP code inside a Blade file.
   ```php
      @php
         $name = 'zach';
      @endphp

      <x-layout name="{{ $name }}">
   ```


# Pass A value in view
   -  Simply create a second argument, then add [], and inside that, you can pass an array or string. 
   Just make sure you use a variable name and value like this: view('home', ['name' => 'Dawn Izach'])
   ```php
      Route::get('/jobs', function(){ 
         return view('jobs', [
            'jobs' => [
               [
                  "id" => 1,
                  "title" => "Manager",
                  "salary" => "60,000"
               ]
               [
                  "id" => 3,
                  "title" => "Programmer",
                  "salary" => "10,000"
               ]
               [
                  "id" => 2,
                  "title" => "Teacher",
                  "salary" => "40,000"
               ]
            ]
         ])    
      })
   ```
   ```php job.blade.php
      <h1>Jobs</h1>
      <ul>
         @foreach($jobs as $job)
            <li>{{$job["title"]}} : {{$job["salary"]}}</li>
         @endforeach
      </ul>
   ```

# Pass A value Specifier in Route then return a specific Page About you specify
   - By adding curly braces like {id} in the route, you can pass a value through the URL and return a 
   specific page based on that value. Example: Route::get('/jobs/{id}', function ($id) { ... }).
   ```php
      Route::get('/jobs/{id}', function ($id) {
         dd($id);
      });
   ```   
   - This will return your id dumpdie method.
   - When you put /jobs/3 in URL address bar you will get the number you put.

# Arr::first() 
   * Arr::first() | Example
   ```php
       public function showjobs($id){
         $jobs = [...];
         $job = Arr::first($jobs, function($job) use ($id){
            return $job["id"] == $id;
         });

         // If you dont want to use use
         $job = Arr::first($jobs, fn($job) => $job['id'] == $id);

      }
   ```
   - Return when the condition is true
   - First will loop all value in array then put it in job 1 by 1 then check the condition and repeat.
   - The value of job will be based on the result of id and the arrays







