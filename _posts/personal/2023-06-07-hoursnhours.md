---
layout: post
title:  "Hours 'n Hours"
summary: "Nobody likes keeping track of worked hours as a freelancer, especially when you keep forgetting to do it. So I made an app to help me."
author: Vincent
date: '2019-05-22 14:35:23 +0530'
category: personal
thumbnail: /assets/img/posts/hoursnhours.jpg
keywords: devlopr jekyll, how to use devlopr, devlopr, how to use devlopr-jekyll, devlopr-jekyll tutorial,best jekyll themes
permalink: /projects/personal/hoursnhours/
usemathjax: true
---

You can find the source code for that app at my [Github](https://github.com/Feathora/hoursnhours)

As a freelancer, you need to keep track of how many hours you've worked. When you're a fulltime freelancer, it's easy. But as a part-time freelancer, who's also a teacher at the HKU, I need to keep track of whether I'm working for the HKU or as a freelancer that day, and how many hours each. Because I kept forgetting to update my excel at the end of every days, I built an app to help me.

```c#
public override void OnReceive(Context context, Intent intent)
{
    var channel = new NotificationChannel("reminders", "Reminder", NotificationImportance.Default);
    channel.EnableVibration(true);
    channel.LockscreenVisibility = NotificationVisibility.Public;

    var manager = (NotificationManager)context.GetSystemService(Context.NotificationService);
    manager.CreateNotificationChannel(channel);

    var builder = new Notification.Builder(context, "reminders").SetContentTitle("Waar heb je vandaag aan gewerkt?").SetAutoCancel(true).SetSmallIcon(Resource.Drawable.tab_about);

    var openAppIntent = new Intent(context, typeof(MainActivity));
    var pendingIntent = PendingIntent.GetActivity(context, 0, openAppIntent, 0);
    builder.SetContentIntent(pendingIntent);

    builder.AddAction(new Notification.Action.Builder(null, "V++", PendingIntent.GetBroadcast(context, 0, new Intent(context, typeof(HoursReceiver)).SetAction("V++"), 0)).Build());
    builder.AddAction(new Notification.Action.Builder(null, "HKU", PendingIntent.GetBroadcast(context, 0, new Intent(context, typeof(HoursReceiver)).SetAction("HKU"), 0)).Build());

    manager.Notify(1, builder.Build());

    var reminderIntent = new Intent(Application.Context, typeof(ReminderReceiver));
    pendingIntent = PendingIntent.GetBroadcast(Application.Context, 0, reminderIntent, PendingIntentFlags.CancelCurrent);

    var reminderTime = new DateTime(DateTime.Now.Year, DateTime.Now.Month, DateTime.Now.Day, 18, 0, 0, DateTimeKind.Local).ToUniversalTime();
    if (reminderTime < DateTime.UtcNow) reminderTime = reminderTime.AddDays(1.0);

    long reminderMillis = (long)(reminderTime - new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc)).TotalMilliseconds;

    (Application.Context.GetSystemService(Context.AlarmService) as AlarmManager).Set(AlarmType.RtcWakeup, reminderMillis, pendingIntent);
}
```

At the end of every day, at 6pm, it pops up a notification on my phone asking me what I've worked on that day. I can then either select V++ or HKU from the notification directly, filling in 8 hours for my choice, or I can open the notification to open the app, allowing me to specify the exact hours for each. Everything is then saved as a JSON file, for me to later parse on my PC and convert into my normal Excel file.
