# Flutter RazorPay

# Getting Started 

This flutter plugin is a wrapper around our Android and iOS SDKs.

The following documentation is only focused on the wrapper around our native Android and iOS SDKs. To know more about our SDKs and how to link them within the projects, refer to the following documentation:

Android: "https://razorpay.com/docs/checkout/android/"

iOS: "https://razorpay.com/docs/ios/"

To know more about Razorpay payment flow and steps involved, read up here: "https://razorpay.com/docs/"

## Prerequisites

1. Learn about the Razorpay Payment Flow.

2. Sign up for a Razorpay Account and generate the API Keys from the Razorpay Dashboard. Using the Test keys helps simulate a sandbox environment. No actual monetary transaction happens when using the Test keys. Use Live keys once you have thoroughly tested the application and are ready to go live.

### Installation

This plugin is available on Pub: "https://pub.dev/packages/razorpay_flutter"

Add this to dependencies in your app's pubspec.yaml

   `razorpay_flutter: ^1.3.4`

Note for Android: Make sure that the minimum API level for your app is 19 or higher.

### Proguard rules

If you are using proguard for your builds, you need to add following lines to proguard files

   `-keepattributes *Annotation*
   -dontwarn com.razorpay.**
   -keep class com.razorpay.** {*;}
   -optimizations !method/inlining/
   -keepclasseswithmembers class * {
   public void onPayment*(...);
   }`

Note for iOS: Make sure that the minimum deployment target for your app is iOS 10.0 or higher. Also, don't forget to enable bitcode for your project.

Run `flutter packages get` in the root directory of your app.

#### Usage

Sample code to integrate can be found in example/lib/main.dart.

Import package

   `import 'package:razorpay_flutter/razorpay_flutter.dart';`

Create Razorpay instance

   `_razorpay = Razorpay();`

Attach event listeners


The plugin uses event-based communication, and emits events when payment fails or succeeds.

The event names are exposed via the constants EVENT_PAYMENT_SUCCESS, EVENT_PAYMENT_ERROR and EVENT_EXTERNAL_WALLET from the Razorpay class.

Use the on(String event, Function handler) method on the Razorpay instance to attach event listeners.

   `
   _razorpay.on(Razorpay.EVENT_PAYMENT_SUCCESS, _handlePaymentSuccess);
   _razorpay.on(Razorpay.EVENT_PAYMENT_ERROR, _handlePaymentError);
   _razorpay.on(Razorpay.EVENT_EXTERNAL_WALLET, _handleExternalWallet);`

The handlers would be defined somewhere as

   `void _handlePaymentSuccess(PaymentSuccessResponse response) {
   // Do something when payment succeeds
   }
   void _handlePaymentError(PaymentFailureResponse response) {
   // Do something when payment fails
   }
   void _handleExternalWallet(ExternalWalletResponse response) {
   // Do something when an external wallet was selected
   }`

To clear event listeners, use the clear method on the Razorpay instance.

   `_razorpay.clear(); // Removes all listeners`

Setup options

   `var options = {
   'key': '<YOUR_KEY_HERE>',
   'amount': 100,
   'name': 'Acme Corp.',
   'description': 'Fine T-Shirt',
   'prefill': {
   'contact': '8888888888',
   'email': 'test@razorpay.com'
   }
   };`

Open Checkout

   `_razorpay.open(options);`


