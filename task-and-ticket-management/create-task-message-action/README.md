# Create task from Message Action

![](create-task.gif)  
*Create task from Message Action*

### Examples

* Create a task from an existing message
* Create a ticket from an existing message

## Required features

* [Bot User](https://api.slack.com/bot-users)
* [Interactive Components](https://api.slack.com/interactive-messages)
* [Message Actions](https://api.slack.com/actions)

## Required scopes

* [`bot`](https://api.slack.com/scopes/bot)

## Implementation overview

### 1. Open modal from Message Action

* [payload.json](payload-task-modal.json)

#### Methods

* [`views.open`](https://api.slack.com/methods/views.open)

### 2. Confirmation in thread

* [payload.json](payload-task-confirm.json)
* [Open in Block Kit Builder](https://api.slack.com/tools/block-kit-builder?blocks=%5B%0A%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22section%22%2C%0A%20%20%20%20%20%20%20%20%20%20%22text%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22mrkdwn%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22text%22%3A%20%22Your%20task%20*Prepare%20customer%20deck*%20has%20been%20created%20%3Aok_hand%3A%22%0A%20%20%20%20%20%20%20%20%20%20%7D%2C%0A%20%20%20%20%20%20%20%20%20%20%22accessory%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22button%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22text%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22plain_text%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22text%22%3A%20%22View%20%3Aarrow_upper_right%3A%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22emoji%22%3A%20true%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22url%22%3A%20%22https%3A%2F%2Fexample.com%22%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%2C%0A%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22divider%22%0A%20%20%20%20%20%20%20%20%7D%2C%0A%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22context%22%2C%0A%20%20%20%20%20%20%20%20%20%20%22elements%22%3A%20%5B%0A%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22mrkdwn%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22text%22%3A%20%22*Assignee%3A*%20%3CfakeLink.toUser.com%7CDavid%3E%22%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22mrkdwn%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22text%22%3A%20%22*Priority%3A*%20%3Aarrow_double_up%3A%20High%22%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22mrkdwn%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%22text%22%3A%20%22*Labels%3A*%20Sales%22%0A%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%5D%0A%20%20%20%20%20%20%20%20%7D%2C%0A%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22section%22%2C%0A%20%20%20%20%20%20%20%20%20%20%22text%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%22type%22%3A%20%22mrkdwn%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22text%22%3A%20%22*Description*%5CnCan%20you%20please%20prepare%20the%20deck%20for%20our%20customer%20meeting%20tomorrow%2C%20%3CfakeLink.toUser.com%7CDavid%3E%3F%5Cn%5Cnhttps%3A%2F%2Fworkspace.slack.com%2Farchives%2FCXXXXXXX%2Fp1553819686001000%22%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%5D)

#### Methods

* [`chat.postMessage`](https://api.slack.com/methods/chat.postMessage)