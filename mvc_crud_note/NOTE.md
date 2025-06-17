CONTINUE:
   [https://www.youtube.com/watch?v=Yi1NfLkflyU]
   1:00:30/1:11:24



# LARAVEL 
   - The three MVC category In Using Laravele
     [model],[view],[controler].
   |               [Controler]
   |             / ˄          ˄ \
   |    update  / /            \ \ update
   |           / /              \ \
   |          / /                \ \
   |         / /                  \ \
   |        / /  notify            \ \
   |       ˅ /          user action \ ˅
   |    [Model]                     [View]
   |̲̲̲̲
      * Controler - User interact with [View], Data intereact with database and to connect user with database 
      |             we have the [controler]
      |           - The work of controler is just to make sure the data will flow between the *view* and the
      |             *model* and `view to model`.
      |           - THe controler design to help view and model to flow the data in each other.
      * Model     - Use to intereact with the database, model will get, save the data and all `CRUD`
      |             relatedpart and some other searching related part will done via [model].
      |           - When you get the data or when you want to save the data everything goes
      |             via controller through [view]
      |       
      * View  - User interact with View


# ✅ Refined Explanation of MVC in Laravel
   - 🧩 Model
      * it's a class, and it represents a table in your database.
      * It can `contain actions (methods)`, `but mostly it handles data, relationships, and business logic`.
      * Each model usually represents one database table (e.g., Book model = books table).
      * It can be related to other models (e.g., a Book belongs to an Author).
      🔹 Think of it as: "Where the data lives and how it's connected."

   - 🎮 Controller
      * Also a class, but its `main job is to handle requests`, interact with the model, and send data to the view.
      * It `doesn't store data` — it `just controls the flow of data`.
      * You can `think of it as the traffic manager`.
      🔹 Think of it as: *"The middleman between data (Model) and display (View)."*

   - 👁️ View
      * This is not a class, but a template (`usually a Blade file`) that shows the data to users.
      * It `contains HTML with some dynamic placeholders ({{ }})` `to display what comes from the controller`.
      🔹 Think of it as: "What the user sees."
   # 🧩 MVC in a Nutshell
      [Model]
      Handles data and business logic (talks to the database).

      [View]
      Displays information and user interface (what the user sees).

      [Controller]
      Receives user input, tells Model what to do, and passes results to View.



#  MVC : Sample
   * 🧠 Model (M) – Handles data and logic
      ```php
         namespace App\Models;

         use Illuminate\Database\Eloquent\Model;

         class Book extends Model
         {
            protected $fillable = ['title', 'author'];
         }
      ```
   * 🎮 Controller (C) – Handles user requests and application logic
      ```php
         namespace App\Http\Controllers;

         use App\Models\Book;

         class BookController extends Controller
         {
            public function index()
            {
               $books = Book::all(); // fetch all books from DB
               return view('books.index', compact('books')); // pass data to view
            }
         }
      ```
   * 👁️ View (V) – Displays data to the user
      ```php
         <!DOCTYPE html>
         <html>
            <head>
               <title>Book List</title>
            </head>
            <body>
               <h1>Books</h1>
               <ul>
                  @foreach($books as $book)
                        <li>{{ $book->title }} by {{ $book->author }}</li>
                  @endforeach
               </ul>
            </body>
         </html>
      ```   
   * 🌐 Route (to connect it all)
   ```php
      use App\Http\Controllers\BookController;

      Route::get('/books', [BookController::class, 'index']);
   ```




# @CSRF   
   - Remember, any HTML forms pointing to [POST], [PUT], [PATCH], or [DELETE] routes that are defined in the web routes file should include a *CSRF token* field. Otherwise, the request will be rejected. 
   ```php
      <form>
         @csrf
      </form>
   ```




















   

