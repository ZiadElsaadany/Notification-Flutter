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
# Notification Handling (onMessage)
Handle notifications received while the app is in the foreground:

```dart
FirebaseMessaging.onMessage.listen((event) {
  Constants.toast("Enter in my app");
  debugPrint(event.data.toString());
});
```
# Sending Notification to Someone by Device Token (FCM Api)
Use the following function to send a notification to a specific device:

```dart
  Future<void> sendFcmMessage() async {
    const String serverKey = 'Server_Key';
    const String deviceToken = 'DEVICE_TOKEN';

    final Map<String, dynamic> notification = {
      'body': 'This is a test FCM message',
      'title': 'FCM Test',
    };

    final Map<String, dynamic> data = {
      'click_action': 'FLUTTER_NOTIFICATION_CLICK',
      'id': '1',
      'status': 'done',
    };

    final Map<String, dynamic> body = {
      'notification': notification,
      'data': data,
      'to': deviceToken,
    };

    const String apiUrl = 'https://fcm.googleapis.com/fcm/send';

    final http.Response response = await http.post(
      Uri.parse(apiUrl),
      headers: <String, String>{
        'Content-Type': 'application/json',
        'Authorization': 'key=$serverKey',
      },
      body: jsonEncode(body),
    );

    print('FCM response: ${response.body}');
  }
```
# Sending Notification to Topic
###  FirebaseMessaging.instance.subscribeToTopic("TOPIC_NAME"),
###  FirebaseMessaging.instance.unsubscribeToTopic("TOPIC_NAME"),
Use the following function to send a notification to a topic:

```dart
Future<void> sendToTopic() async {
  const String serverKey = 'Server_key';

  final Map<String, dynamic> notification = {
    'body': 'This is a test FCM message',
    'title': 'FCM Test',
  };

  final Map<String, dynamic> data = {
    'click_action': 'FLUTTER_NOTIFICATION_CLICK',
    'id': '1',
    'status': 'done',
  };

  final Map<String, dynamic> body = {
    'notification': notification,
    'data': data,
    'to': "/topics/topic_name",
  };

  const String apiUrl = 'https://fcm.googleapis.com/fcm/send';

  final http.Response response = await http.post(
    Uri.parse(apiUrl),
    headers: <String, String>{
      'Content-Type': 'application/json',
      'Authorization': 'key=$serverKey',
    },
    body: jsonEncode(body),
  );

  print('FCM response: ${response.body}');
}
```




