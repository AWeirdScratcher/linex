<br />

<div align="center">

<picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://github.com/AWeirdScratcher/linex/assets/90096971/befa28f0-f5a5-4962-b58e-dd08c7cee002">
    <source media="(prefers-color-scheme: light)" srcset="https://github.com/AWeirdScratcher/linex/assets/90096971/a5687590-f8d3-4feb-b452-e405918c72c7">
    <img alt="LineX By Ramptix" src="https://github.com/AWeirdScratcher/linex/assets/90096971/a5687590-f8d3-4feb-b452-e405918c72c7">
</picture>

<div><br /></div>

The fast and easy-to-use LINE bot SDK. Focus on the content, we do the rest.

<!-- CTA button -->

<a href="/">
    <picture>
        <source media="(prefers-color-scheme: dark)" srcset="https://github.com/AWeirdScratcher/linex/assets/90096971/8b69a4f5-6c4b-410c-8f88-92cfe06a73a0" width="150">
        <source media="(prefers-color-scheme: light)" srcset="https://github.com/AWeirdScratcher/linex/assets/90096971/ae2669a3-4b2c-4a14-929a-9b42caef3571" width="150">
        <img alt="LineX By Ramptix" src="https://github.com/AWeirdScratcher/linex/assets/90096971/ae2669a3-4b2c-4a14-929a-9b42caef3571" width="150">
    </picture>
</a>

<img
    src="https://github.com/AWeirdScratcher/linex/assets/90096971/b435ae9a-a4d2-4689-a171-3978951983d7"
    alt="pip install -U linex"
    width="510"
/>

[![By Ramptix](https://img.shields.io/badge/%E2%AC%9C%20%E2%94%82%20By%20Ramptix-%23202020?style=for-the-badge)](https://github.com/ramptix)

</div>

## Features

It's (or it has):

- Based on [FastAPI](https://fastapi.tiangolo.com)
- Feature-rich
- Async-ready
- Around 60% coverage of the LINE API [(Contribute)](https://github.com/AWeirdScratcher/linex/fork)
- Better type hints than [linelib](https://github.com/AWeirdScratcher/linelib) (my previous work)

<sub>(Not affiliated with The X Corp.)</sub>

## Feel The Simplicity

Code snippets explain more than words. Take a look:

```python
from linex import Client, TextMessageContext

client = Client("channel secret", "channel access token")

@client.event
async def on_ready():
    print(f"Logged in as {client.user.name}")

@client.event
async def on_text(ctx: TextMessageContext):
    await ctx.reply("Hello, World!")

client.run()
```

That's it. Say no more to additional setups — they're so annoying!

## Documentation

Currently, there is no documentation for LineX — yet I'm working on it. In the meantime, type hints and code editors are the best documentation sources.

## Extensions

<div align="center">

### Notify Support
![Notify Banner](https://scdn.line-apps.com/n/line_notice/img/pc/img_lp02_en.png)


</div>

LineX also supports LINE notify, including push message sending and OAuth2.

Here's a simple notify bot:

```python
from linex.ext import Notify

notify = Notify("access token")

notify.notify_sync(
    "Hello, World!"
)
```

### Locale

Have users that use different languages? Try out the `Locale` extension.

First, define a file structure like so:

```haskell
i18n/
├─ _meta.json
├─ food.json
```

Inside of `_meta.json`, define the available locales:

```json
{
    "locales": ["en-US", "zh-Hant"]
}
```

The locales above are just examples.

Then, create any JSON file with a specific topic (category) under the directory that stores locale strings.

In this case, `food.json` would look like:

```json
{
    "pizza": {
        "en-US": "pizza",
        "zh-Hant": "披薩"
    },
    "describe": {
        "en-US": "{food} tastes good!",
        "zh-Hant": "{food} 很好吃！"
    }
}
```

The first key (`pizza`) and the second one (`describe`) stores locale strings with keys defined in the `locales` field in `_meta.json`.

Additionally, if you add texts surrounded by `{}`, it would be considered as an argument that's ready to be passed in. In the above example, `"{food} tastes good!"`, implies that the `{food}` field would be replaced with a food name later in the Python code.

Finally, define our LINE bot:

```python
from linex import Client
from linex.ext import Locale

client = Client("channel secret", "channel access token")
locale = Locale(
    "i18n", # the locale strings directory
    sorted_by="categories" # files split by categories
)

@client.event
async def on_text(ctx):
    loc = await locale(ctx) # get locale for this user
    
    await ctx.reply(
        loc(
            "food/describe", # use <category>/<key name> to get
            food=loc("food/pizza") # the argument (pizza)
        )
    )

client.run()
```

***

(c) 2023 AWeirdScratcher (AWeirdDev)
