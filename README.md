# Android-Notification-Oreo
This repository describes how you can implement notifications in Android Oreo and also backward compatible with previous android versions

## 1) Create Notification Channel
```java
// Create the NotificationChannel, but only on API 26+ because
// the NotificationChannel class is new and not in the support library
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
  String name = "channel name";
  String description = "channel description";
  int importance = NotificationManager.IMPORTANCE_DEFAULT;
  NotificationChannel channel = new NotificationChannel(CHANNEL_ID, name, importance);
  channel.setDescription(description);
  // Register the channel with the system; you can't change the importance
  // or other notification behaviors after this
  NotificationManager notificationManager = getSystemService(NotificationManager.class);
  notificationManager.createNotificationChannel(channel);
}
```

## 2) Create an explicit intent for an Activity in your app
```java
Intent intent = new Intent(this, MainActivity.class);
intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK | Intent.FLAG_ACTIVITY_CLEAR_TASK);
PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, intent, 0);
```
## 3) Set the Notification Content
```java
NotificationCompat.Builder mBuilder = new NotificationCompat.Builder(this,CHANNEL_ID)
  .setSmallIcon(R.drawable.icon_sm)
  .setContentTitle("This is notification Title")
  .setContentText("This is notification Message or Text")
  .setContentIntent(pendingIntent) //Pending Intent For Tap Action
  .setPriority(NotificationCompat.PRIORITY_DEFAULT)
  .setAutoCancel(true);
```

## 4) Show the Notification
```java
NotificationManagerCompat notificationManager = NotificationManagerCompat.from(this);
// notificationId is a unique int for each notification that you must define
notificationManager.notify(NOTIFICATION_ID, mBuilder.build());
```
