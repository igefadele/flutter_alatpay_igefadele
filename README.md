# AlatPay Payment SDK

The AlatPayFlutter class is a flutter widget that allows you to integrate WEMA Bank's Alatpay payment gateway into your flutter project. It provides a WebView that loads an Alatpay payment page with the transaction details to be processed. The class handles the response from the payment gateway and provides callback functions for handling successful transactions and errors. The class requires parameters such as metaData, businessId, apiKey, and amount to process the payment. It supports Android, iOS and Web platforms. Other platforms coming soon.

## 👨💻 Architect & Author
**Ige Fadele** — *Principal AI Systems Architect & Machine Learning Engineer*  
🌐 **Website & Portfolio:** [igefadele.savadub.com](https://igefadele.savadub.com) | 🏫 **Institute:** [institute.savadub.com](https://institute.savadub.com) | 💼 **LinkedIn:** [linkedin.com/in/igefadele](https://linkedin.com/in/igefadele) | 🐙 **GitHub:** [github.com/igefadele](https://github.com/igefadele)  
🐦 **X (Twitter):** [@igefadele](https://x.com/igefadele) | 📺 **YouTube:** [@igefadele](https://youtube.com/@igefadele) | 📸 **Instagram:** [@igefadele](https://instagram.com/@igefadele) | 🎵 **TikTok:** [@igefadele](https://tiktok.com/@igefadele) | 👤 **Facebook:** [facebook.com/igefadele](https://facebook.com/igefadele)

## Features

* Integration with Alatpay Payment Gateway: The AlatPayFlutter class allows you to easily integrate the AlatPay payment gateway into your Flutter app.

* WebView Integration: The class provides a WebView that loads the Alatpay's payment page with the transaction details to be processed.

* Transaction Handling: The class handles the response from the payment gateway and provides callback functions for handling successful transactions and errors.

* Secure Transactions: The class generates a secure hash required by alatpay to ensure secure transactions.

* Customizable: The class is customizable and allows you to configure parameters such as apiKey, businessId, currency, amount, and metaData to process the payment.

Overall, the AlatPayFlutter class provides a convenient and secure way to integrate the AlatPay payment gateway into your Flutter app

## Documentation

`businessId` : The business ID obtained from AlatPay.

`amount` : The amount to be charged for the transaction.

`apiKey` : The api key obtained from AlatPay.

`onTransaction` : A callback function that is called when the payment is completed successfully.

`onClose`: A callback function that is called when the service is closed.

<!-- `alatpay_flutter:` AlatPayFlutter dependency.

`AlatPayFlutter`: The main AlatPayFlutter widget that handles the whole process. -->

## Installation

To use this package, add `alatpay_flutter` as a [dependency in your pubspec.yaml file](https://flutter.io/platform-plugins/).

```yaml {"id":"01HJZ0C4E8NYR3TDB3TYMVYFWD"}
dependencies:
  alatpay_flutter: latest version
```

## Example

How to use the alatpay_flutter package.

```dart {"id":"01HJZ0C4E8NYR3TDB3V1EYNWDG"}

import 'dart:io';

import 'package:alatpay_flutter/alatpay_flutter.dart';
import 'package:flutter/material.dart';
import 'package:get/route_manager.dart';

import 'screens/success.dart';
import 'src/constants.dart';
import 'src/controllers.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'AlatPay',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: kAccentColor),
        useMaterial3: true,
      ),
      home: const PaymentScreen(),
    );
  }
}

class PaymentScreen extends StatefulWidget {
  const PaymentScreen({Key? key}) : super(key: key);

  @override
  State<PaymentScreen> createState() => _PaymentScreenState();
}

class _PaymentScreenState extends State<PaymentScreen> {
//============= Boolean variables ===============\\
  bool isLoading = false;

//============= Function ===============\\
  void pay() {
    String apiKey = "alatPayPrimaryKey";
    String businessId = "alatPayBuinessId";
    String email = "johndoe@gmail.com";
    String phone = "+2348047589684";
    String firstName = "John";
    String lastName = "Doe";
    String currency = 'NGN';
    String amount = "20000";

    setState(() {
      isLoading = true;
    });

    //=========== Not so important ===========\\

    // Map meta = {
    //   "the_order_id": "order_id",
    //   'client_id': "user_id"
    // };

    //=========== End =============\\
    try {
      Navigator.push(
        context,
        MaterialPageRoute(builder: (context) {
          return AlatPayFlutter(
            apiKey: apiKey,
            businessId: businessId,
            email: email,
            phone: phone,
            firstName: firstName,
            lastName: lastName,
            currency: currency,
            amount: amount,
            // metaData: meta,
            onClose: () {
              Get.close(2);
            },
            onTransaction: (response) async {
              //============ Do something =============\\
              Get.to(const SuccessScreen());
            },
          );
        }),
      );
      setState(() {
        isLoading = false;
      });
    } on SocketException {
      ApiProcessorController.errorSnack("Please connect to the internet");
    } catch (e) {
      consoleLog(e.toString());
    }
    setState(() {
      isLoading = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        elevation: 10,
        title: Text(
          "AlatPay",
          style: TextStyle(
            color: Theme.of(context).appBarTheme.backgroundColor,
            fontSize: 20,
            fontWeight: FontWeight.w600,
          ),
        ),
      ),
      body: SafeArea(
          child: Center(
        child: ElevatedButton(
          onPressed: pay,
          style: ElevatedButton.styleFrom(
            disabledBackgroundColor: Colors.grey.shade300,
            backgroundColor: Theme.of(context).primaryColor,
            elevation: 10,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(20),
            ),
            shadowColor: Colors.black.withOpacity(0.4),
          ),
          child: isLoading
              ? const Padding(
                  padding: EdgeInsets.all(10),
                  child: CircularProgressIndicator(color: Colors.white),
                )
              : const Text(
                  "Pay now",
                  textAlign: TextAlign.center,
                  style: TextStyle(
                    color: Colors.white,
                    fontSize: 18,
                    fontFamily: "Sen",
                    fontWeight: FontWeight.w700,
                  ),
                ),
        ),
      )),
    );
  }
}


```
