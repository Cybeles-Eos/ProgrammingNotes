   Need MOA if under na ng PUP no need
   Need DTI 0r SEC Registered HTEs from company

# Notes through internet.


   [laravel_docs]:
   [_syntax][request#accessing-the-request]
   - Retrieving the Request Method.
      **[ ->isMethod() ]** 🎇
      * The method method will `return the HTTP verb for the request`. You may use the isMethod method to verify that the HTTP verb matches a given string:
      ```php
         $method = $request->method();

         if ($request->isMethod('post')) {
            // ...
         }
      ```
      _N_: This isMethod same as $_SERVER["REQUEST_METHOD"]
      
      **[ ->input() ]** 🎇
      * Access the input from form data
      ```php
         public function store(Request $request){
            $name = $request->input('name');
            $nameWithDefault = $request->input('name', 'Guess');
            
            // Store the user...
      
            return redirect()->route('home');
         }
      ```
      * You can set a second argument for default name.
      _N_: The $request is from your form data.

      **[ ->all() | access-index-array | laravel-request-object ]** 🎇
      * You may retrieve all of the incoming request's input data as an array using the all method
      ```php
         $input = $request->all();

         //To access index value use array access.
         $name = $input[products][0][name]; 
         
         $name = $request->input('products.0.name');
         // Means For all name attribute you will access the first name value.
      ```
      _N_: This will get all data in your form and store in array.
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   
   [laravel_docs]:
   [_syntax][request#accessing-the-request]








































