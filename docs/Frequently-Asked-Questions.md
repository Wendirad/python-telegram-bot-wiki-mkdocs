# Frequently Asked Questions

*Note:* You may also want to check the official [Telegram Bot FAQ](https://core.telegram.org/bots/faq#what-messages-will-my-bot-get).

### What messages can my Bot see?

From the official [Telegram Bot FAQ](https://core.telegram.org/bots/faq#what-messages-will-my-bot-get):
***

> **All bots, regardless of settings, will receive:**
>
> * All service messages.
> * All messages from private chats with users.
> * All messages from channels where they are a member.
>
> **Bot admins and bots with privacy mode disabled will receive all messages except messages sent by other bots.**
> 
> **Bots with privacy mode enabled will receive:**
> 
> * Commands explicitly meant for them (e.g., /command@this_bot).
> * General commands from users (e.g. /start) if the bot was the last bot to send a message to the group.
> * Messages sent via this bot.
> * Replies to any messages implicitly or explicitly meant for this bot.
> 
> **Note that each particular message can only be available to one privacy-enabled bot at a time, i.e., a reply to bot A containing an explicit command for bot B or sent via bot C will only be available to bot A. Replies have the highest priority.**
***

### What about messages from other Bots?
***
> Bots talking to each other could potentially get stuck in unwelcome loops. To avoid this, we decided that bots will not be able to see messages from other bots regardless of mode.
>
***

### Can my bot delete messages from the user in a private chat?

Yes, but only within the first 48 hours.

### How can I get a list of all chats/users/channels by bot is interacting with?

There is no method for that. You'll need to keep track.

### How do I send messages to all my bots users?

Broadcasting to your users is a common use case. Please have a look at this short [article](https://telegra.ph/Sending-notifications-to-all-users-07-17).

### Does my bot get an update, when someone joins my channel?

No. Those service messages are available only in groups.

### My bot doesn't receive messages from groups. Why?

See [here](#What-messages-can-my-Bot-see?). TL;DR: Disable group privacy with [@BotFather](https://t.me/BotFather)

### Can you add [feature] to PTB? Can I do [thing] with my bot?

Please note that python-telegram-bot is only a *wrapper* for the Telegram Bot API, i.e. PTB can only provide methods that are available through the API.
You can find a full list of all available methods in the [official docs](https://core.telegram.org/bots/api#available-methods).
Anything *not* listed there can not be done with bots. Here is a short list of frequently requested tasks, that can *not* be done with the Bot API:

* Getting a list of all members of a group
* Adding members to a group/channel (note that you can just send an inivite link, which is also less likely to be seen as spam)
* Clearing the chat history for a user
* Getting a message by its `message_id`
* Getting the last sent message in a chat (you can keep track of that by using [`chat_data`](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Storing-bot,-user-and-chat-related-data))

In some cases, using a userbot can help overcome restrictions of the Bot API. Please have a look at this [article](http://telegra.ph/How-a-Userbot-superacharges-your-Telegram-Bot-07-09) about userbots.
Note that userbots are not what python-telegram-bot is for.

### I'm using `ConversationHandler` and want to one handler to be run multiple times. How do I do that?

If your handlers callback returns `None` instead of the next state, you will stay in the current state. That means the next incoming update can be handled by the same callback.

### I want to handle updates from an external service in addition to the Telegram updates. How do I do that?

Receiving updates from an external service, e.g. updates about your GitHub repo, is a common use case.
How exactly you get them sadly is beyond the scope of PTB, as that depends on the service. For many cases a simple approach is to check for updates every x seconds. You can use the [`JobQueue`](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Extensions-–-JobQueue) for that.

If you have a setup for getting the updates, you can put them in your bots update queue via `updater.update_queue.put(your_update)`. The `update_queue` is also available as `dispatcher.update_queue` and `context.update_queue`.
Note that `your_update` does *not* need to be an instance of `telegram.Update` - on the contrary! You can e.g. write your own custom class to represent an update from your external service.
To actually do something with the update, you can register a [`TypeHandler`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.ext.typehandler.html). [`StringCommandHandler`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.ext.stringcommandhandler.html) and [`StringRegexHandler`](https://python-telegram-bot.readthedocs.io/en/stable/telegram.ext.stringregexhandler.html) might also be interesting for some use cases. 

### Why am I getting `ImportError: cannot import name 'XY' from 'telegram'`?

You probably have a file named `telegram.py` or a directory/module named `telegram` in your working directory. This leads to namespace issues.
Rename them to something else.

### What do the `per_*` setting in `ConversationHandler` do?

`ConversationHandler` needs to decide somehow to which conversation an update belongs.
The default setting (`per_user=True` and `per_chat=True`) means that in each chat each user can have its own conversation - even in groups.
If you set `per_user=False` and you start a conversation in a group chat, the `ConversationHandler` will also accept input from other users.
Conversely, if `per_user=True`, but `per_chat=False`, its possible to start a conversation in one chat and continue with it in another.

`per_message` is slightly more complicated: Image two different conversations, in each of which the user is presented with an inline keyboard with the buttons yes and no.
The user now starts *both* conversations and sees *two* such keyboards. Now, which conversation should handle the update?
In order to clear this issue up, if you set `per_message=True`, the `ConversationHandler` will use the `message_id` of the message with the keyboard.
Note that this approach can only work, if all the handlers in the conversation are `CallbackQueryHandler`s. This is useful for building interactive menus.

### Can I check, if a `ConversationHandler` is currently active for a user?

There is no built-in way to do that. You can however easily set a flag as e.g. `context.user_data['in_conversation'] = True` in your `entry_points`s and set it to `False` before returning `ConversationHandler.END`.

### How can I list all messages of a particular chat or search through them based on a search query?

There is no API method for that (see [here](#can-you-add-feature-to-ptb-can-i-do-thing-with-my-bot)). If you really need this functionality, you'll need to save all the messages send to the chat manually. Keep in mind that

1. In group chats your bot doesn't receive all messages, if privacy mode is enabled (see [here](#what-messages-can-my-bot-see))
2. Messages may be edited (in which case your bot will receive a corresponding update)
3. Messages may be deleted (and there are no updates for "message deleted"!)
