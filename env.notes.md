# What is .env File and What you need to learn.
   - ✅ The Most Important .env Variables You Should Know.

   * [APP_NAME]:
      _Purpose_: Name of your app (used in notifications, titles, etc.)
      _Example_: - APP_NAME="Liteshuttle"

   * [APP_ENV]:
      _Purpose_: Defines environment (local, production, etc.)
      _Example_: APP_ENV=production

   * [APP_DEBUG]:
      _Purpose_: Shows detailed errors (true for dev, false for live)
      _Example_: - APP_DEBUG=false

   * [APP_URL]:
      _Purpose_: Base URL of your site
      _Example_: APP_URL=https://liteshuttle.montecarlowebgraphics.com

   * [ASSET_URL]:
      _Purpose_: Custom base for assets (like CDN or full HTTPS URL)
      _Example_: ASSET_URL=https://liteshuttle.montecarlowebgraphics.com

   🔐 Security-Related
      * [APP_KEY]:
      _Purpose_: Used to encrypt data securely (must be set)
      _Example_: APP_KEY=base64:...
      * [APP_DEBUG=false]:
      _Purpose_: Prevents showing error details to users
      _Example_: (Always false in production)

   💾 Database Connection
   | Variable        | Purpose               | Example                    |
   | --------------- | --------------------- | -------------------------- |
   | `DB_CONNECTION` | Database type         | `mysql`                    |
   | `DB_HOST`       | Database server       | `127.0.0.1` or `localhost` |
   | `DB_PORT`       | Port to connect to DB | `3306`                     |
   | `DB_DATABASE`   | Database name         | `liteshuttle_db`           |
   | `DB_USERNAME`   | DB user               | `root`                     |
   | `DB_PASSWORD`   | DB password           | `secret`                   |










# What you need to do in .env before uploading it in https or domain.
   - In your [AppServiceProvider.php], add this in the boot() method to `force Laravel to use HTTPS` for `generated URLs`.
   - This will change the request from http to https. Means This will be a secured url link.
   [App/Providers/AppServiceProvider.php]
   ```php 
      use Illuminate\Support\Facades\URL;

      public function boot()
      {
         if (env('APP_ENV') === 'production') {
            URL::forceScheme('https');
         }
      }
   ```
   - Then you need to change some code in .env file to be able to work the assets and style in your domain
   [.env]
   ```
      APP_NAME="Lite Shuttle" 
      APP_ENV=production
      APP_KEY=base64:uvST973o9VzKEy1JvWlXXzRmho5qhaz1M7hy7PaQ+Cs=
      APP_DEBUG=true
      APP_URL=https://liteshuttle.montecarlowebgraphics.com
      ASSET_URL=https://liteshuttle.montecarlowebgraphics.com

      LOG_CHANNEL=stack

      DB_CONNECTION=mysql
      DB_PORT=3306
      DB_DATABASE=lr_liteshuttle_dv
      DB_USERNAME=mcfg
      DB_PASSWORD="WjSRsVL7%jc9"
   ``` 
   1. You will ne to change the DATABASE PORT, DATABASE NAME, USERNAME and PASSWORD
      - This changes is for your import sql in phpadmin inside your created DATABASE NAME
   2. You Will need to change the  APP_ENV, APP_URL and ASSET_URL.
      - APP_ENV : You will change this from local to production
                * local : Live site for real users |Development
                * production : Live site for real users |Live/Deployed
      - APP_URL : defines the base URL of your Laravel application.
      - ASSET_URL : used specifically to override the default base URL for static 
                    assets like images, CSS, and JS files.


