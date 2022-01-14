# notification system

## Design scope
### Types
* Push notifications
* SMS
* Email
### Supported devices
* iOS
* Android
* laptop
### Others
* Soft-realtime system
* Notifications can be triggered by client applications
* Enable to opt-out
* Capability: (10 million mobile push notifications, 1 million SMS messages, and 5 million emails) / day.

## Third-party notification service

### App Push Notification Service, APNS
* iOS devices
* Payload and Device token
### Firebase Cloud Message Service, FCMS
* Android devices (iOS is available)
* Demo on firebase console

### SMS
* Twilio
* Nexmo 
### Email
* Sendgrid...

## Databse schema
A user can have multiple devices, indicating that a push notification can be sent to all the user devices.
Table: *user*(id, email, ...)
Table: *device*(id, device_token, last_login_at...)

(But I don't thinks device table is needed because most of current practice will use topic-subscription.)

## Architecture
![](https://i.imgur.com/ycJcEih.jpg)
### Multiple notification server (internal only)
avoiding issues like:
* Single point of failure, SPOF
* Performace bottleneck
* Scalability
#### rate limitor
To avoid overwhelming users with too many notifications, we can limit the number of notifications a user can receive.
### Database and cache
For better horizontal scaling.
### Message queue
Will decouple system components.
### Notification template
avoid building every notification from scratch.
### Notification log
Improve reliability, this notification log database is included for data persistence, prevent data loss.

## Improvement
### Retry mechanism
When a third-party service fails to send a notification, the notification will be added to the message queue for retrying. If the problem persists, an alert will be sent out to developers.

### Monitor queued notifications
A key metric to monitor is the total number of queued notifications. If the number is large, the notification events are not processed fast enough by workers. To avoid delay in the notification delivery, more workers are needed.

## Integrated Cloud service
* [AWS SNS service](https://aws.amazon.com/tw/sns/?did=ft_card&trk=ft_card&whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc)








