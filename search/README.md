# Blueprint: Perform search action

![](create-task.gif)  
*Perform search action*

### Examples

* Perform a search action with a Slash Command

## Required App functionality

* [Bot User](https://api.slack.com/bot-users)
* [Interactive Components](https://api.slack.com/interactive-messages)
* [Slash Commands](https://api.slack.com/slash-commands)

### Show results from Slash Command

* [payload.json](payload-results.json)

#### Methods

* [`chat.postMessage`](https://api.slack.com/methods/chat.postMessage)

### Edit search

* [payload.json](payload-edit-search.json)
* [Open in Block Kit Builder]()

#### Methods

* [`chat.update`](https://api.slack.com/methods/chat.update)

## List of required scopes

* [`bot`](https://api.slack.com/scopes/bot)
* [`commands`](https://api.slack.com/scopes/commands)

## Recommended usage

| Message Type  | Recommended |
| ------------- | ------------- |
| Public Channel | :white_check_mark: | 
| Private Channel | :white_check_mark: | 
| Thread | :white_check_mark: |
| DM | :white_check_mark: |
| Group DM | :white_check_mark: |
