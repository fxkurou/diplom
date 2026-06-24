[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

# LoL in-house bot
A Discord bot to handle League of Legends in-house games, with role queue, matchmaking, and rankings.

# Installation

- Install [Docker](https://docs.docker.com/get-docker/)

- Get your Discord bot token from [the Discord developer portal](https://discord.com/developers/applications)

- Activate your bot on the Discord developer portal and give it the Server Members privileged intent

- Invite the bot to your server through OAuth2

- Add emoji for all 5 LoL roles to your server

    - They are handled separately than champion emoji as they’re crucial for the bot to work

    - Optional: invite your bot to servers that have emoji for each champion, for example :TwistedFate: for Twisted Fate and :KaiSa: for Kai’Sa. You can also define a :loading: emoji that will be used by the bot 

- Create a `docker-compose.yml` file based [on this docker compose file](https://github.com/fxkurou/diplom/blob/master/docker-compose-example.yml)

- Edit the file to add your Discord bot token as well as the Discord ID of your emojis, and change the database default password to something random

     - You can add the environment variable `INHOUSE_BOT_TEST=1` to the bot’s variables and it will add a few `!test` commands

     - You can add the environment variable `INHOUSE_BOT_COMMAND_PREFIX` to customize the prefix of the bot (will default to `!`).

- Run `docker-compose up -d` and your bot should be up and running!

    - If you also added the `adminer` service, you can use http://localhost:8080/ to manage the database
    
- Use `!admin mark queue` to define queue channels

# Rating and matchmaking explanation

Rating:
- Each player has one rating per server and role, and each rating is completely independent
- There is one queue per discord channel the bot is in, but ratings are server-wide
- The ratings are loosely based on [Microsoft TrueSkill](https://en.wikipedia.org/wiki/TrueSkill)
- The displayed MMR is a conservative estimate of skill and starts at 25 for everybody

Matchmaking:
- Players who have been in queue the longest will be favored when creating a game
- Matchmaking aims to select the game with a predicted winrate as close as possible to 50%
- Side assignment is random

# Use case and behaviour

This bot is made to be used by trustworthy players queuing regularly for one or two roles. It will not transfer well to
an uncontrolled environment.

Players can queue in multiple channels and multiple roles. A game starting will drop them from 
all queues in all channels. A player can’t re-enter a queue as long as any game they’re in has not been scored or 
cancelled.