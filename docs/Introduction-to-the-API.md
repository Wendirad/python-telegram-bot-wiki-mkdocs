The API is exposed via the [telegram.Bot](https://github.com/python-telegram-bot/python-telegram-bot/blob/master/telegram/bot.py) class. The methods are the snake_case equivalents of the methods described in the official [Telegram Bot API](https://core.telegram.org/bots/api). The exact camelCase method names as in the Telegram docs are also available for your convenience. So for example `telegram.Bot.send_message` is the same as `telegram.Bot.sendMessage`.

To generate an Access Token, you have to talk to [BotFather](https://t.me/botfather) and follow a few simple steps (described [here](https://core.telegram.org/bots#6-botfather)).

For full details see the official Telegram documentation at [Bots: An introduction for developers](https://core.telegram.org/bots).

#### Hello, Telegram!

To get a feeling for the API and how to use it with `python-telegram-bot`, **please open a Python command line** and follow the next few steps.

First, create an instance of the `telegram.Bot`. `'TOKEN'` should be replaced by the API token you received from `@BotFather`:

```python
>>> import telegram
>>> bot = telegram.Bot(token='TOKEN')
```

To check if your credentials are correct, call the [getMe](https://core.telegram.org/bots/api#getme) API method:

```python
>>> print(bot.get_me())
{"first_name": "Toledo's Palace Bot", "username": "ToledosPalaceBot"}
```

**Note:** Bots can't initiate conversations with users. A user must either add them to a group or send them a message first. People can use ``telegram.me/<bot_username>`` links or username search to find your bot.

#### What to read next?
To get real and start building your first bot using the `telegram.ext` classes, read [Extensions – Your first Bot](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Extensions-–-Your-first-Bot)

If you want to continue learning about the API, read [Code snippets](https://github.com/python-telegram-bot/python-telegram-bot/wiki/Code-snippets).