# Flutter Firebase Cloud Messaging (FCM) Setup and Usage

This repository contains a Flutter project demonstrating the setup and usage of Firebase Cloud Messaging (FCM) for sending and receiving push notifications in a Flutter app.

## Get Token for Device to Send Notification

To obtain the FCM token for a device, use the following code:

```dart
Future<String?> getTokenFromFirebase() async {
  return await FirebaseMessaging.instance.getToken();
}
