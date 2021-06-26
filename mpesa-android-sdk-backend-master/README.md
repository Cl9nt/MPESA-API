# mpesa-android-sdk-backend
Backend for the [MPESA android sdk](https://github.com/bdhobare/mpesa-android-sdk)

# Installing laravel
Follow [this](https://www.howtoforge.com/tutorial/install-laravel-on-ubuntu-for-apache/) short and easy tutorial to install Laravel on your server.

1. Clone this project and upload it to your server's webroot
2. Edit `.env` file to connect the project to your database.
3. Run `composer update` to initialize the project
4. Run the migrations: `php artisan migrate`
5. Configure Apache to point to the `/public` folder. [Guide](http://laravel-recipes.com/recipes/25/creating-an-apache-virtualhost)
6. If you are using Firebase to send push notifications, edit your `.env` file and add this line
 ``FIREBASE_SERVER_KEY=YOUR_SERVER_KEY``
7. On the mobile app, call this method, replacing `url` with the link to the backend
   ```builder.setCallBackURL(http://url/mpesa)```
8. You are set.
