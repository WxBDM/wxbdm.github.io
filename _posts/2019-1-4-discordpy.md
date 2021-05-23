---
layout: post
title: Creating a Discord bot using discord.py Rewrite
---

Discord is a platform designed for creating communities. It utilizes text and voice channels in a "server". In these servers, users can invite bots to aid in server management, to help users with a specific task, or to entertain users with a text-based game.

This discord bot that is built in this guide will have a few fun commands: 8ball, poll, and randomly selecting a quote from The Office. Note that this guide will NOT show you how to set up your bot, but rather give you the code for the bot in full with discussion, where appropriate.

### Code: The Bot

Note that for both 8 ball and The Office quotes, the data is loaded into a database.

```python

import discord
import random
import pandas as pd
import sqlite3

banned_words = ['noob', 'n00b']

8ball_answers = ['Don't count on it.', 'My reply is no.', 'My sources say no.', 'Outlook not so good.', 'Very doubtful.', 'It is certain.', 'It is decidedly so.', 'Without a doubt.', 'Yes – definitely.', 'You may rely on it.', 'As I see it, yes.', 'Most likely.', 'Outlook good.', 'Yes.', 'Signs point to yes.', 'Reply hazy, try again.', 'Ask again later.', 'Better not tell you now.', 'Cannot predict now.', 'Concentrate and ask again.']

client = commands.Bot(command_prefix = "~")

@client.event
async def on_ready():

	print(f"Bot running. ID: {client.user.id}")

@client.event # whenever a user invokes a message
async def on_message():
	
	# have this here so the bot doesn't respond to itself
	if message.author == client.user:
		return
	
	# check to see if any word is in the banned words.
	if message.content.lower() in banned_words:
		await message.channel.send("That's a bad word!")
	
	await bot.process_commands(message)


@client.command() # Sends an 8ball message.
async def 8ball(ctx):

	phrase = 8ball_answers = random.randint(0, len(8ball_answers))
	async ctx.send(phrase)

@client.command() # puts either a thumbs up or thumbs down on the message.
async def poll(ctx, *, text : str = None):

	if text != None:
		await ctx.message.add_reaction("\U0001f44d")
		await ctx.message.add_reaction("\U0001f44e")


```


