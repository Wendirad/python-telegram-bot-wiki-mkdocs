You've now made a cool bot, but it's lacking personality? Add some emoji!

# Direct Method

The easiest way to use emoji is to directly put them in your strings. The Unicode website has a chart with [all the emoji](http://www.unicode.org/emoji/charts/full-emoji-list.html). Simply select any emoji you want, this works with both the images and the raw characters in the "Brow." column, and paste it in your string.

```python
text = "🌈⛈🎉🌹🐧😊"
```

In the code you may see squares with numbers in them instead of the emoji themself. This means the font in your text editor does not have an image for that character, but it is still there.

This will work without problems on Python 3. On Python 2 you need to declare the encoding of your source file, put this line at the top:
```python 
# -*- coding: utf-8 -*-
```
this tells Python that your source file is encoded in UTF8. Note that if you have a shebang at the top, the encoding line comes second:
```python 
#!/usr/bin/env python
# -*- coding: utf-8 -*-
```

Finally, test your emoji by sending it to yourself over Telegram. Know that Telegram does not support all the emoji.

# The emoji module

With the [emoji module](https://github.com/carpedm20/emoji) you don't have to copy paste emoji, you can use their names or aliases as on GitHub:
```python
from emoji import emojize
bot.send_message(emojize("yummy :cake:", use_aliases=True))
```

Note: the `emojize` function uses regular expressions and takes on the order of microseconds to complete. If your bot handles billions of messages per second, put the emoji in reusable variables to micro-optimize:
```python
cake = emojize(":cake:", use_aliases=True)
```