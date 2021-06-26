# MPESA ANDROID SDK

This SDK allows android developers to easily make MPESA payments on their apps.

# Screenshots

# Features

The SDK includes the following:

1. Android native SDK to accept MPESA payments in less than 10 lines of code.

2. A sample backend in Laravel you can easily deploy to your server to quickly get started.

3. A sample DB schema for the backend.

4. Integration with Firebase Cloud Messaging to easily send push notifications to the app after payments.

5. A complete sample application you can easily run to see the SDK in action.

6. A free [hosted backend] you can use for testing purposes.

7. This nice README 😀

## Getting started

To get started you need the following from the [Safaricom Developer dashboard].

1. Your Consumer Key.

2. Your Consumer Secret.

3. The business shortcode.

4. A Firebase project with a server key.


For testing purposes, you can get test credentials [here](https://developer.safaricom.co.ke/test_credentials).

Use the *Lipa Na Mpesa Online Shortcode* and *Lipa Na Mpesa Online Passkey* from the link.

## Integrating with your app

1. Add the SDK to your project.
    
2. Initialize the SDK

        Mpesa.with(context, CONSUMER_KEY, CONSUMER_SECRET).
    
        You can optionally pass the mode as the third parameter , either `SANDBOX` or `PRODUCTION`.
    
        Mpesa.with(context, CONSUMER_KEY, CONSUMER_SECRET, Mode.PRODUCTION).

3. Implement the `AuthListener` interface in your activity.The interface provides two methods

        public void onAuthError(Pair<Integer, String> result).

        This method is called when initializing the Mpesa instance fails.
        You can get the response code using `result.code` and the error message using `result.message`.
        Make sure your credentials are correct.

        public void onAuthSuccess()``` 

        This method is called once the Mpesa instance initializes successfully.
        You can only make a successful transaction after this method is called.Therefore, you can use this method to update the user interface like enabling a disabled button.

## Making a transaction

Before making a transaction, your need to make sure you have the following items:

1. A business shortcode.

2. A pass key. For testing, you can get one from the same link.

3. The amount to transact.

4. The phone number making the payment. It can be in 07xx.. or 254xx.. or +254xx... formats. The SDK will sanitize it for you.

        To make a transaction , you build an `STKPush` instance and pass it to `pay()` method
    
        STKPush.Builder builder = new Builder(BUSINESS_SHORT_CODE, PASSKEY, amount,BUSINESS_SHORT_CODE, phone).
    
        STKPush push = builder.build().
    
        Mpesa.getInstance().pay(context, push).

5. Make sure your activity implements `MpesaListener`. The interface provides two methods you will need to implement :

        Public void onMpesaError(Pair<Integer, String> result).
 
        This method is called if the request could not be processed. You can get the error message generated by using `result.message`
 
        Public void onMpesaSuccess(String MerchantRequestID, String CheckoutRequestID, String CustomerMessage).
 
        This method is called once the request has been received by Safaricom and accepted for processing.The user will be presented with the prompt to enter their MPESA pin.

Congrats! You have successfully made an MPESA transaction.

## Customising the transaction

Most of the times you want to receive the transaction details on your server, store them in your database and then send a confirmation notification to the user.

I have include a complete working backend in Laravel you can easily deploy to your server.

For running the sample project, you don't need to deploy your own backend.
### Setting up the backend

1. Clone the backend from [here](https://github.com/cl9nt-m/mpesa-android-sdk/mpesa-android-sdk-backend.git).If you are new to Laravel you can quickly catch up [here](https://www.parthpatel.net/laravel-tutorial-for-beginner-5-4/).

2. Upload the repository to your server and set up the necessary configurations(Apache, Composer, Laravel, MySQL/PostgreSQL etc).

3. Create a sample database in Postgresql or Mysql and connect it with the project by editing the `.env` file.

4. Run the migrations : `php artisan migrate` from the project root.

5. Get the url to your backend. I am going to assume you have deployed it to `http://YOUR_URL`.

6. Set up the callback url to the `STKPush.Builder` by calling this method:

        builder.setCallBackURL(http://YOUR_URL/mpesa).

## Please note that the url is `YOUR_URL/mpesa` not `YOUR_URL`.  `/mpesa` is a route in the backend

Once the above steps are finished, the response from Safaricom will be sent to the backend and inserted into the database.

### Setting up Firebase Notifications

1. To send a confirmation notification to the user, you need to add Firebase to your app by following this short guide [here](https://firebase.google.com/docs/cloud-messaging/android/client).

    **The sample app already has a fully working Firebase implementation.You can copy paste the code to avoid repeating the process. You only need to download your google-services.json file and put it in the app/ folder.**

2. Get your server key from the Firebase Dashboard by going to Project Settings >> Cloud Messaging.

3. Login into your backend server and open the `.env` file. Add this line to the file.

        FIREBASE_SERVER_KEY=YOUR_SERVER_KEY.

4. On your android app, pass the user's Firebase token  to `STKPush.Builder` by calling this method.

        Builder.setFirebaseRegID(token).
    
        This token is not the server's key from Step 2. It is the one got from `onTokenRefresh()` on the user's phone.

    *Note: This firebase token will be appended to the callback url to get a url of the format `http://YOUR_URL/mpesa/token`*

## Going live

To go live you only need to pass one extra parameter when initializing the SDK

        Mpesa.with(context, CONSUMER_KEY, CONSUMER_SECRET, Mode.PRODUCTION).
        
As it is with the sandbox, you need to implement `AuthListener` interface.
    
## Running the Sample Project

1. To build the sample project, clone this project, open it in Android Studio and replace the credentials in  `MainActivity.java` with yours from the Safaricom dashboard.

2. Create a Firebase project as described above and download your `google-services.json` file to the `app/` folder.

3. Hit RUN in Android Studio.

## Contributing and Issues

The project is really young and I would appreciate any pull requests or bug reports.Use [EMAIL](muthunguclintn@gmail.com) for bug reports or email me directly.
