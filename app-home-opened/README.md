# Blueprint: Onboarding (`app_home_opened` event)

![](app-home-opened.gif)  
*`app_home_opened` event example*

🎥 [High Resolution screencast](app-home-opened.mp4)

💻 [Check out code example on Glitch](https://glitch.com/~app-home-opened)

## Description

Your Slack app lives in a virtual place called an App Home. Users find it by locating or adding your app in their channel sidebar, under the Apps heading.

Your App Home is essentially a private, 1:1 conversation between a person and your bot user. It works just like a direct message between two users, using the same events and Web API methods.

By enabling the `app_home_opened` event you are able to greet users with interactive, educational experiences and unlock the full potential of your App Home.

More information around App Home can be found on our [API docs](https://api.slack.com/reference/app-home).

_Please note that this event only works in your app's DM._

### Examples

* Onboarding new users once they open your App's DM for the first time
* Send a help message with important Call-to-actions once they open your App's DM

## Required features

* [Bot User](https://api.slack.com/bot-users)
* [Events API](https://api.slack.com/events-api)

## Required scopes

* [`bot`](https://api.slack.com/scopes/bot)

## Required event subscriptions

* [`app_home_opened`](https://api.slack.com/events/app_home_opened)

## Implementation overview

### 1. Receive `app_home_opened` event

This app event notifies your app when a user has entered the App Home.

Your Slack app must have a bot user configured and installed to use this event.

If the user opens a tab within the App Home, the event payload for this event will reference that in the tab field. If they opened a Home tab, a view field will also be included, containing the current state of that Home tab, including the list of blocks, and various pieces of metadata.

#### Event

* [`app_home_opened`](https://api.slack.com/events/app_home_opened)

#### Event payload

```
POST /your-event-subscription-url HTTP/1.1
User-Agent: Slackbot 1.0 (+https://api.slack.com/robots)
Content-Type: application/json
X-Slack-Signature: v0=xxxxxx
X-Slack-Request-Timestamp: 1575670714

{
  "team_id": "TXXXXX",
  "api_app_id": "AXXXXXX",
  "event": {
    "type": "app_home_opened",
    "user": "UXXXXXX",
    "channel": "DXXXXXX",
    "tab": "messages",
    "type": "event_callback",
    "event_id": "EXXXXXX",
    "event_time": 1575670714
  }
}
```

### 2. Send a welcome message if users has opened your App's DM for the first time

To determine if a user opens your message tab for the first time, you can either
1. track the `app_home_opened` event on your side.
2. use the [`im.history`](https://api.slack.com/methods/im.history) and query the history for existing messages. If the history is empty, you can assume a user has opened the messages tab for the first time.

* [payload.json](payload-welcome.json)
* [Open in Block Kit Builder](https://api.slack.com/tools/block-kit-builder?mode=message&blocks=%5B%7B%22type%22%3A%22section%22%2C%22text%22%3A%7B%22type%22%3A%22mrkdwn%22%2C%22text%22%3A%22Hey%20there%20%F0%9F%91%8B%20I%27m%20TaskBot.%20I%27m%20here%20to%20help%20you%20create%20and%20manage%20tasks%20in%20Slack.%5CnThere%20are%20two%20ways%20to%20quickly%20create%20tasks%3A%22%7D%7D%2C%7B%22type%22%3A%22section%22%2C%22text%22%3A%7B%22type%22%3A%22mrkdwn%22%2C%22text%22%3A%22*1%EF%B8%8F%E2%83%A3%20Use%20the%20%60%2Ftask%60%20command*.%20Type%20%60%2Ftask%60%20followed%20by%20a%20short%20description%20of%20your%20tasks%20and%20I%27ll%20ask%20for%20a%20due%20date%20(if%20applicable).%20Try%20it%20out%20by%20using%20the%20%60%2Ftask%60%20command%20in%20this%20channel.%22%7D%7D%2C%7B%22type%22%3A%22section%22%2C%22text%22%3A%7B%22type%22%3A%22mrkdwn%22%2C%22text%22%3A%22*2%EF%B8%8F%E2%83%A3%20Use%20the%20_Create%20a%20Task_%20action.*%20If%20you%20want%20to%20create%20a%20task%20from%20a%20message%2C%20select%20%60Create%20a%20Task%60%20in%20a%20message%27s%20context%20menu.%20Try%20it%20out%20by%20selecting%20the%20_Create%20a%20Task_%20action%20for%20this%20message%20(shown%20below).%22%7D%7D%2C%7B%22type%22%3A%22image%22%2C%22title%22%3A%7B%22type%22%3A%22plain_text%22%2C%22text%22%3A%22Create%20a%20task%22%2C%22emoji%22%3Atrue%7D%2C%22image_url%22%3A%22https%3A%2F%2Fapi.slack.com%2Fimg%2Fblocks%2Fbkb_template_images%2FonboardingComplex.jpg%22%2C%22alt_text%22%3A%22image1%22%7D%5D)

#### Methods

* [`chat.postMessage`](https://api.slack.com/methods/chat.postMessage)

#### API Call

```
POST https://slack.com/api/chat.postMessage HTTP/1.1
Content-Type: application/json
Authorization: Bearer xoxb-xxxxxx-xxxxxx

{
  "channel": "DXXXXXX",
  "text": "This text shows up in the notification.",
  "blocks": [...]
}
```

#### API Response

```
HTTP/1.1 200 
Content-type: application/json; charset=utf-8

{
  "ok": true,
  "channel": "DXXXXXX",
  "ts": "1575672707.000200",
  "message": {
    "type": "message",
    "subtype": "bot_message",
    "text": "This text shows up in the notification.",
    "ts": "1575672707.000200",
    "username": "Your bot username",
    "bot_id": "BXXXXXX",
    "blocks": [
      ...
    ]
  }
}
```

### 2a. Request `im.history`

To query the history for existing messages, you can use 

#### Methods

* [`im.history`](https://api.slack.com/methods/im.history)

#### API Call

```
POST https://slack.com/api/im.history HTTP/1.1
Content-Type: application/json
Authorization: Bearer xoxb-xxxxxx-xxxxxx

{
  "channel": "DXXXXXX",
  "text": "This text shows up in the notification",
  "blocks": [...]
}
```


