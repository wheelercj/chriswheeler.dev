+++
title = 'Making discord bots'
date = 2025-03-12T20:14:50-07:00
lastmod = 2025-03-16T20:57:59-07:00
tags = []
+++

Discord bots run as web services that use Discord's API. They listen for requests from Discord and send responses. They need their own databases if a way to persist data is necessary. However, some Discord bot features require saving some data about the bot in Discord's servers, such as slash command names.

Automated testing of Discord bots tends to be very limited or nonexistent since Discord has rate limits and is too complex to practically mock. Creating Discord bots tends to require a lot of trial and error, but speeds up as you learn. It's fun since you can get a simple bot up and running fairly quickly, and the visual feedback you get from each improvement can be very satisfying. Now that I've built a multi-purpose Discord bot, I've been able to create new, specialized bots within only a few days each when the opportunity arises since I mostly understand the API and have code to copy.

There are Discord API wrappers for many languages. The one I'm familiar with is [Rapptz/discord.py: An API wrapper for Discord written in Python](https://github.com/Rapptz/discord.py).

- [discord.py documentation](https://discordpy.readthedocs.io/en/stable/index.html)
- Searching [the discord.py Discord server](https://discord.gg/dpy) for examples and answers is frequently helpful
- [scarletcafe/jishaku: A debugging and testing cog for discord.py bots](https://github.com/scarletcafe/jishaku)

Regardless of which tools you use, creating a Discord bot requires creating the bot's account in [the Discord Developer Portal](https://discord.com/developers/applications).

## Project structure

There are multiple ways to structure the code for bots written with discord.py, but I'll describe the way that seems most common among well-written bots. The entrypoint is `main.py`, which only connects to the database, creates an instance of the bot, and starts the bot. In `bot.py`, a `Bot` class is defined that subclasses `commands.Bot`. This class has no commands. It only handles global events like logging, error handling, cooldowns (command rate limits), and managing _cogs_.

### Cogs

Cogs are like modules that group related commands together. They can be loaded, unloaded, and reloaded while the bot is running. Most Discord bots only take a few seconds to start up, but some take 30 minutes or even more because of how many servers they're in. For those, being able to change a cog's code and reload the cog without restarting the bot can be very helpful. A cog that many bots have is `Owner`, which groups all commands that only the bot's owner can use. By subclassing `commands.Cog` and defining the `cog_check` method, you can easily keep your administrative commands secure:

```py
import os
import sys
from discord.ext import commands  # https://pypi.org/project/discord.py/


class Owner(commands.Cog):
    """Commands that can only be used by the bot owner."""

    def __init__(self, bot) -> None:
        self.bot = bot

    async def cog_check(self, ctx):
        if not await self.bot.is_owner(ctx.author):
            raise commands.NotOwner
        return True

    @commands.hybrid_command()
    async def restart(self, ctx):
        """Restarts the bot"""
        await ctx.send("Restarting")
        python = sys.executable
        os.execl(python, python, *sys.argv)


async def setup(bot):
    await bot.add_cog(Owner(bot))
```

## Next steps

> One must learn by doing the thing, for though you think you know it, you have no certainty until you try.
> 
> — Sophocles, 5th century B.C.

That's why the rest of this post—except the examples at the end—will just be very specific tips and tricks.

## Interactions

An [interaction](https://discordpy.readthedocs.io/en/stable/interactions/api.html) must be responded to exactly once and within 3 seconds, or an error will occur. If more time and/or multiple responses will be needed, you should use [defer](https://discordpy.readthedocs.io/en/stable/interactions/api.html#discord.InteractionResponse.defer), and then [followup](https://discordpy.readthedocs.io/en/stable/interactions/api.html#discord.Interaction.followup) within 15 minutes.

As of 2025-03-12, modals can only contain text inputs, and views cannot contain text inputs. A single modal can have up to 5 text inputs. A single select (a dropdown menu) can have up to 25 options. As far as I know, Discord's developers have not said whether these will ever change.

The `long` and `paragraph` text input styles are identical both visually and functionally.

### Syncing slash commands

To add slash commands to a bot, after writing the code for them, you need to sync the commands to Discord. In other words, you have to save some info about the slash commands in Discord's servers, such as the command names.

Syncing slash commands is somewhat confusing and error-prone. Discord has some rules such as a limit on the number of slash commands a bot can have, and a limit on the total combined length of all slash command names, cog names, descriptions, etc. If these limits are exceeded, the problem can be difficult to diagnose. A while ago, I wrote [a command](https://github.com/wheelercj/Parhelion/blob/3355b2374fa75a2aae1009d601e9e94488b66175/cogs/owner.py#L406) that searches for problems that would make syncing fail; I haven't been trying to keep it up to date, but it might still be correct. There's also a rate limit on syncing commands; you can only attempt syncing every once in a while, and Discord tends to be vague about what their rate limits are.

When I sync slash commands for the first time, I start with one of [jishaku](https://github.com/scarletcafe/jishaku)'s commands: `jishaku sync .` (notice the period). This command syncs server commands to the current server. It can detect some problems and give feedback. If that works, I then sync global commands to the current server with [a command I wrote](https://github.com/wheelercj/Parhelion/blob/3355b2374fa75a2aae1009d601e9e94488b66175/cogs/owner.py#L487) that's fairly standard. From there, you can test your slash commands more, sync global commands globally, etc.

If you end up with two of each slash command, try removing the bot from the server and adding it again.

### Ephemeral messages

Messages can be ephemeral, which means that they can only be seen by their sender and receiver, and will eventually disappear. Bots can only send ephemeral messages in response to [interactions](https://discordpy.readthedocs.io/en/stable/interactions/api.html), such as slash commands.

You can send an ephemeral message with `await ctx.send("Secret message", ephemeral=True)`. However, you must not use it with `async with ctx.typing():` or else the message will not be ephemeral. Apparently, using `ctx.typing` counts as sending a non-ephemeral message, and `ctx.send` kind of "edits" that "message", but cannot edit whether it is ephemeral. Fortunately, interactions already show a loading indicator anyways, so using `ctx.typing` should not be necessary.

### Interaction exception handling

Any exceptions raised in view or modal callbacks are passed to that object's `on_error` method, but nowhere else. It is recommended to subclass `discord.ui.View`, implement `on_error`, and subclass that view so you can handle all view errors in one place. Same for modals.

## Examples

- [Rapptz's discord.py examples](https://github.com/Rapptz/discord.py/tree/master/examples)
- [Rapptz/RoboDanny](https://github.com/Rapptz/RoboDanny)
- [Parhelion](https://github.com/wheelercj/Parhelion), a Discord bot I made
- [Retrobot](https://github.com/rossimo/retrobot), which "allows you to play NES/SNES/GB/GBA games with your friends over chat" by creating and sending GIFs
- [Harmon758/Harmonbot: Multi-Platform Factotum Bot](https://github.com/Harmon758/Harmonbot)
- [IAmTomahawkx/bob: An advanced moderation bot for discord, allowing for extreme flexibility through TOML based guild configuration](https://github.com/IAmTomahawkx/bob)
- [dredd-bot/Dredd: A multipurpose Discord bot written in python language and enhanced discord.py library.](https://github.com/dredd-bot/Dredd)
- [EmoteBot/EmoteCollector: Collects emotes from other servers for use by people who don't have Nitro](https://github.com/EmoteBot/EmoteCollector)
- [daggy1234/dagbot: The official Repository for dagbot, the self proclaimmed n1 meme bot.](https://github.com/Daggy1234/dagbot)
- [albertopoljak/Licensy: Discord bot that manages expiration of roles with subscriptions!](https://github.com/albertopoljak/Licensy)
- [zachsamuels/FoodBot: A food bot for discord](https://github.com/zachsamuels/FoodBot)
- [lambda x / Cautious Memory · GitLab](https://owo.codes/lambda/cautious-memory)
- [StarrFox/Discord-chan: A Discord chat bot](https://github.com/StarrFox/Discord-chan)
- [niztg/CyberTron5000: A discord bot](https://github.com/niztg/CyberTron5000)
- [aaronhnsy/life-bot](https://github.com/aaronhnsy/life-bot)
- [daggy1234/MemeMixer: A fun meme generator game involving 3 people. It's a bot for discord.](https://github.com/Daggy1234/MemeMixer)
- [lgaan/server-pets: A discord bot written in python](https://github.com/lgaan/server-pets)
- [karx1/YashBot3001: Discord bot written with discord.py](https://github.com/karx1/YashBot3001)
- try searching [the discord.py server](https://discord.gg/dpy) with `!source` to find more
