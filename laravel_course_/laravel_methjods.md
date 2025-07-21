# Laravel Methods



Reference: [https://www.youtube.com/watch?v=SqTdHCTWqks&t=7220s]
[LearnLaravel] - [2:02:00]


# use Illuminate\Support\Arr;
| Method                 | Description                                                      |
| ---------------------- | ---------------------------------------------------------------- |
| `Arr::add()`           | Add a key/value to array if the key doesn't exist.               |
| `Arr::collapse()`      | Collapse an array of arrays into a single array.                 |
| `Arr::crossJoin()`     | Generate the cross join of arrays.                               |
| `Arr::divide()`        | Divide array into two arrays: keys and values.                   |
| `Arr::dot()`           | Flatten a multi-dimensional array with dot notation.             |
| `Arr::except()`        | Return array excluding specified keys.                           |
| `Arr::exists()`        | Check if key exists (even if value is null).                     |
| `Arr::first()`         | Return the first element passing a truth test.                   |
| `Arr::flatten()`       | Flatten multi-dimensional array into a single-level array.       |
| `Arr::forget()`        | Remove item(s) using "dot" notation.                             |
| `Arr::get()`           | Retrieve a value using "dot" notation.                           |
| `Arr::has()`           | Check if item exists using "dot" notation.                       |
| `Arr::hasAny()`        | Check if any of the given keys exist.                            |
| `Arr::isAssoc()`       | Determine if array is associative.                               |
| `Arr::isList()`        | Determine if array is a list (sequential numeric keys).          |
| `Arr::join()`          | Join array values with a string (with optional final separator). |
| `Arr::keyBy()`         | Key the array by the value of a given key or callback.           |
| `Arr::last()`          | Return the last element passing a truth test.                    |
| `Arr::map()`           | Map callback over each item, returning a new array.              |
| `Arr::only()`          | Return only specified keys.                                      |
| `Arr::pluck()`         | Pluck an array of values from an array.                          |
| `Arr::prepend()`       | Prepend an item to an array.                                     |
| `Arr::pull()`          | Get a value and remove it from the array.                        |
| `Arr::query()`         | Build a query string from an array.                              |
| `Arr::random()`        | Get one or more random items.                                    |
| `Arr::set()`           | Set a value using "dot" notation.                                |
| `Arr::shuffle()`       | Shuffle the items randomly.                                      |
| `Arr::sort()`          | Sort the array using values.                                     |
| `Arr::sortRecursive()` | Recursively sort the array.                                      |
| `Arr::toCssClasses()`  | Convert array into a CSS class string.                           |
| `Arr::undot()`         | Expand a dot-notated array into a nested one.                    |
| `Arr::where()`         | Filter array by a truth-test callback.                           |
| `Arr::whereNotNull()`  | Remove items with `null` values.                                 |
| `Arr::wrap()`          | Wrap a value in an array if it's not already an array.           |


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



































