+++
title = 'Making discord bots'
date = 2025-03-12T20:14:50-07:00
lastmod = 2025-03-26T17:25:04-07:00
tags = []
+++

Discord bots run as web services that use Discord's API. They listen for requests from Discord and send responses. They need their own databases if a way to persist data is necessary. However, some Discord bot features require saving some data about the bot in Discord's servers, such as slash command names.

Automated testing of Discord bots tends to be very limited or nonexistent since Discord has rate limits and is too complex to practically mock. Creating Discord bots requires a lot of trial and error, but speeds up as you learn. It's fun since you can get a simple bot up and running fairly quickly, and the visual feedback you get from each improvement can be very satisfying. Now that I've built a multi-purpose Discord bot, I've been able to create new, specialized bots within only a few days each when the opportunity arises since I mostly understand the API and have code to copy.

There are Discord API wrappers for many languages. The one I'm familiar with is [Rapptz/discord.py: An API wrapper for Discord written in Python](https://github.com/Rapptz/discord.py).

- [discord.py documentation](https://discordpy.readthedocs.io/en/stable/index.html)
- Searching [the discord.py Discord server](https://discord.gg/dpy) for examples and answers is frequently helpful
- [scarletcafe/jishaku: A debugging and testing cog for discord.py bots](https://github.com/scarletcafe/jishaku)

Regardless of which tools you use, creating a Discord bot requires creating the bot's account in [the Discord Developer Portal](https://discord.com/developers/applications). Setting up your own local test instance of a bot you're working on makes manual testing and debugging much easier. For each bot I work on, I create an extra Discord bot account in the Discord Developer Portal for testing.

## Project structure

There are multiple ways to structure the code for bots written with discord.py, but I'll describe the way that seems most common among well-written bots. The entrypoint is `main.py`, which only connects to the database, creates an instance of the bot, and starts the bot. In `bot.py`, a `Bot` class is defined that subclasses `commands.Bot`. This class has no commands. It only handles global events like logging, error handling, cooldowns (command rate limits), and managing _cogs_. Cogs are in a folder named `cogs`, which has a folder named `utils` containing various utilities shared among the cogs.

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

## Type annotations and docstrings

Python's type annotations and docstrings usually have no runtime effect, but discord.py uses them for some features. As an example, I copied part of [wheelercj/GitHub-bot](https://github.com/wheelercj/GitHub-bot)'s `/issue list` command below. The `list_issues` method has three parameters after the `ctx` parameter: `repo_name`, `assignee`, and `state`. Since these are parameters of a command's function, they are parameters of the command. They each have a default value, so they are all optional. The `Literal["open", "closed", "all"]` tells Discord to require one of those three strings to be entered if the user enters anything for the `state` option. The `repo_name`, `assignee`, and `state` parameters are described in the docstring following [numpydoc's docstring standard](https://numpydoc.readthedocs.io/en/latest/format.html#docstring-standard). Discord takes those descriptions and displays them when the user is entering options if the command is used as a slash command. They also appear in command help pages if you aren't using slash commands.

```py
    @commands.hybrid_group()
    async def issue(self, ctx: commands.Context):
        """A group of commands for managing GitHub issues"""

    @issue.command(name="list", aliases=["ls"])
    async def list_issues(
        self,
        ctx: commands.Context,
        repo_name: str | None = None,
        assignee: str | None = None,
        state: Literal["open", "closed", "all"] = "open",
    ):
        """Shows a repo's GitHub issues

        Parameters
        ----------
        repo_name: str | None
            Filter repos by name, or part of the name.
        assignee: str | None
            Filter issues by the GitHub user assigned.
        state: Literal["open", "closed", "all"]
            Only show "open", "closed", or "all" issues. Defaults to "open".
        """
```

## Next steps

> One must learn by doing the thing, for though you think you know it, you have no certainty until you try.
> 
> — Sophocles, 5th century B.C.

There's a lot about Discord bots that I'm not covering here because you will learn so much faster with experimentation, looking at examples like the ones at the end of this page, and asking questions. That's why the rest of this post will just be very specific tips and tricks. If you're new to working on Discord bots and you're joining an existing project, it may be worth it to create a small Discord bot of your own so you understand more of the framework.

## Interactions

An [interaction](https://discordpy.readthedocs.io/en/stable/interactions/api.html) must be responded to exactly once and within 3 seconds, or an error will occur. If more time and/or multiple responses will be needed, you should use [defer](https://discordpy.readthedocs.io/en/stable/interactions/api.html#discord.InteractionResponse.defer), and then [followup](https://discordpy.readthedocs.io/en/stable/interactions/api.html#discord.Interaction.followup) within 15 minutes. If you have a `ctx` available, you can use `await ctx.defer()` (with `ephemeral=True` if you want the first deferred response to be ephemeral), and `await ctx.send(your_message)` will followup. Modals cannot be deferred.

As of 2025-03-12, modals can only contain text inputs, and views cannot contain text inputs. A single modal can have up to 5 text inputs. A single select (a dropdown menu) can have up to 25 options. As far as I know, Discord's developers have not said whether these will ever change.

The `long` and `paragraph` text input styles are identical both visually and functionally.

### Syncing slash commands

To add slash commands to a bot, after writing the code for them, you need to sync the commands to Discord. In other words, you have to save some info about the slash commands in Discord's servers, such as the command names.

If your bot uses [jishaku](https://github.com/scarletcafe/jishaku), you can use:

- `jsk sync $` to sync all global slash commands globally
- `jsk sync .` to sync any slash commands that are just for the current Discord server
- `jsk sync *` to sync server slash commands to all known servers

Jishaku can give you feedback in most cases if there's a problem with your slash command data. Discord has a bunch of rules about slash commands, such as how long the command descriptions are. There's also a rate limit on syncing commands, and Discord is usually vague about what their rate limits are.

If you ever end up with two of each slash command, there are a few things you can try that might help:

- press `Ctrl+R` (Mac: `Cmd+R`) to reload Discord
- kick your bot from the server and add it again
- wait a minute to see if Discord's servers were just temporarily inconsistent
- try syncing again, but remember that there's a rate limit
- if you somehow synced global commands globally **and** global commands to the current server, sync the current server's commands to the current server (I'm not sure if this problem is possible with Jishaku's sync commands)

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
