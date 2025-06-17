# Facades Class components. 

   Top 12 Most Common Laravel Facades
   | **Facade**  | **Usage** Example                                 | **Purpose**                                |
   | ----------- | ------------------------------------------------- | ------------------------------------------ |
   | `DB`        | `DB::table('users')->get();`                      | Run raw SQL queries, use query builder     |
[.]| `Route`     | `Route::get('/home', ...)`                        | Define app routes                          |
   | `Auth`      | `Auth::check()`, `Auth::user()`                   | User login, check if user is authenticated |
   | `Validator` | `Validator::make($data, [...])`                   | Validate form data                         |
   | `Hash`      | `Hash::make($password)`                           | Hash or verify passwords                   |
   | `Storage`   | `Storage::disk('local')->put('file.txt', 'data')` | File upload/download (local, s3, etc.)     |
   | `Log`       | `Log::info('Something happened')`                 | Write to logs                              |
   | `Session`   | `Session::put('key', 'value')`                    | Store/retrieve session data                |
   | `Cache`     | `Cache::get('key')`, `Cache::put(...)`            | Caching values for speed                   |
   | `Mail`      | `Mail::to($user)->send(new WelcomeMail())`        | Send emails                                |
   | `Response`  | `return Response::json([...])`                    | Return JSON or other HTTP responses        |
   | `Redirect`  | `return Redirect::route('home')`                  | Redirect to route or URL                   |




























