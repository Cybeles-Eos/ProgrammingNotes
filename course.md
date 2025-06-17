# 11 hours LARAVEL FULL COURSE
   - [https://www.youtube.com/watch?v=0M84Nk7iWkA]
   - 21:38 / 10:54:50



# 1hour :|  Learn Laravel 11 Basics in 1 Hour
   - [https://www.youtube.com/watch?v=Yi1NfLkflyU]


- How to create laravel projects
   - First download the composer then in cmd check if the composer succesesfully 
   download
   - DL path to xampp/php/php.exe
   - check composer in cmd
   ```
      > composer
   ```

   * Then create your laravel inside htdocs in xampp
   ```
      projects>$  composer create-project laravel/laravel first-laravel
   ```
      - create a folder called first-laravel
      - then copy the contents of laravel framework repository on github that folder
      - And any third-party PHP packages that laravel depends on are also downloaded
      and saved into that folder(first-laravel)

      _Note_: If this not working or have problem, go to php.ini or config file
      then open it in vscode or notepad then uncomment the ;extension=zip to extension=zip
      ```php.ini config  
         ;extension=zip
      ```
      ```php.ini config   ✅
         extension=zip
      ```

   - How to run your laravel project
   ```
      php artisan serve 
   ```