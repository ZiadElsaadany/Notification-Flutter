# Flutter Firebase Cloud Messaging (FCM) Setup and Usage

This repository contains a Flutter project demonstrating the setup and usage of Firebase Cloud Messaging (FCM) for sending and receiving push notifications in a Flutter app.

## Get Token for Device to Send Notification

To obtain the FCM token for a device, use the following code:

```dart
Future<String?> getTokenFromFirebase() async {
  return await FirebaseMessaging.instance.getToken();
}
```
# AndroidManifest.xml Configuration
Add the following `<intent-filter>` to your main activity in `AndroidManifest.xml`:

```dart
 <intent-filter>
        ho <action android:name="com.google.firebase.MESSAGING_EVENT" />
</intent-filter>
<intent-filter>
    <action android:name="FLUTTER_NOTIFICATION_CLICK" />
    <category android:name="android.intent.category.DEFAULT" />
</intent-filter>
```
# Background Notification Handling
Handle background notifications by registering a callback function:

```dart
FirebaseMessaging.onBackgroundMessage(firebaseNotificationBackgroundHandler);

Future<void> firebaseNotificationBackgroundHandler(RemoteMessage remoteMessage) async {
  // Handle background notification
  debugPrint("on background");
  Constants.toast("on background");
  if (kDebugMode) {
    print(remoteMessage.messageId);
  }
}
```
# Notification Click Handling
Handle notification clicks when the app is opened:

```dart
FirebaseMessaging.onMessageOpenedApp.listen((event) {
  Constants.toast("Open App");
  debugPrint("On Home");
  debugPrint(event.data.toString());
});
```


