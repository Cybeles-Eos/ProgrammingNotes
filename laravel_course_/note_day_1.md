Reference: [https://www.youtube.com/watch?v=SqTdHCTWqks&t=1362s]
[LearnLaravel] - [22:33]
            
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


































