---
title: "How to code your own discord bot!"
date: 2022-02-11T11:19:06-05:00
draft: false
author: "Said Neder"
tags: ["Javascript"]
toc: true
image: "images/botdemo.gif"
categories: "Coding"
---

# Patrick Star Bot 📕

![GitHub License](https://img.shields.io/github/license/crazyc4t/PatrickStar?color=brightgreen)
![GitHub last commit](https://img.shields.io/github/last-commit/crazyc4t/PatrickStar)

This is my first time programming a bot on discord with discord.js!
This bot is about having my classes at my fingertips, so it
gives links with special commands that facilitate the process.
You are free to clone the code and use it the way you want!
You can edit it and make it your own!

![Demo](/images/botdemo.gif)

## Commands

**Prefix: 9**

The prefix means that is the initial character that is used to communicate
with the bot!

|Command|Functionality|
|-----|---- |
9info | Information about the project
9help | Shows all the commands available
9mail | Sends a gmail link
9idukay| Sends a idukay link (learning platform from my school)
9day| It shows the classes of the day

## Want to edit it for yourself? Follow along!
The code itself is documented so go check it out [here!](bot.js)

let's do this step by step!

### Requirements

- [Node JS](https://nodejs.org/en/)
  - if you are on linux use this fast command!
  - Arch Linux: `sudo pacman -S nodejs`
  - Debian/Ubuntu: `sudo apt install nodejs`
  - Redhat/Fedora: `sudo dnf install nodejs`
- [npm](https://www.npmjs.com/) (Already a Node JS dependency)
- [git](https://git-scm.com/) (Already installed on Linux)

And some knowledge on javascript!

**Guide yourself with the official discord.js [docs](https://discordjs.guide/#before-you-begin)!**

## Before we begin

We need to clone this repo with this command:
```bash
git clone https://github.com/crazyc4t/PatrickStar.git
```
And then to install the discord js library with a simple npm command!

```bash
npm install discord.js
```
create a new application in the developer portal of discord, [over here!](https://discord.com/developers/applications),
once you go there create a new app and invite it to your server!

After that, open the project folder in your favourite text editor and let's start!
### Logging into discord

Now in your folder project where you have your bot.js file, and create a `config.json` that is where
we are going to store sensitive data so we can ignore it in a `.gitignore` file.

Then in your config.json should look something like this:

```json
{
  "prefix": "X",

  "token": "X",

  "channelid": "X",

  "guildid": "X"
}

```
What you are going to store:
- Prefix = That is the initial character to know that the message is for your block
- Token = Is your bot discord token, you can copy it from the developer [portal](https://discord.com/developers/applications)
- ChannelID = Is the channel where your bot is going to reply (usually the spam channel from every server)
you can find it activating the developer settings in discord's settings and then right-clicking the channel.
- GuildID = Is the server ID

**Now we will start with the javascript Code:**

```javascript
// require the discord.js and cron module
const Discord = require("discord.js");
const cron = require("cron");

// create a new Discord client
const client = new Discord.Client();

// remote config file where my data is stored
const { prefix, token, channelid, guildid } = require("./config.json");

// When client is ready, run this:
// This will only run one time after logging in
client.once("ready", () => {
  console.log(`Online as ${client.user.tag}`);
  client.user.setActivity("9help");
  const kalamardo = client.guilds.cache.get(guildid);
  const channel = kalamardo.channels.cache.get(channelid);

```

First we start by declaring the variables of the modules we are going to need,
then we create the discord client with the variable of `client` and that is what we are going
to use to refer to discord.

Then we create the variables from the `config.json` to use them in the javascript file.
The `client.once` is a discord.js function that is going to execute something just once when the bot is once ready,
as in this bot we use it to tell us that is running with the `client.user.tag` that is the name of our bot,
and we declare the variables of the channel where we want to send the messages.

### Setting up Cron
Cron is a npm library that allows us to run something on scheadule, we will use it
to notify us everytime we have class!
- [docs](https://www.npmjs.com/package/cron)

to install it we need to execute:

`npm install cron`

Now we are going to create every cron job to run when the bot once log in:

```javascript
const notificationclass = `You have class! GO NOW ${idukay}`;

const classbreak = "You have a break right now, patience and be calm, enjoy!";
// send a message if it is the time specified.
let classnotification1 = new cron.CronJob("00 00 13 * * 1-5", () => {
  channel.send(notificationclass);
});

let classnotification2 = new cron.CronJob("00 35 13 * * 1-5", () => {
  channel.send(notificationclass);
});

let classnotification3 = new cron.CronJob("00 10 14 * * 1-5", () => {
  channel.send(notificationclass);
});

let classnotification4 = new cron.CronJob("00 45 14 * * 1-5", () => {
  channel.send(classbreak);
});

let classnotification5 = new cron.CronJob("00 00 15 * * 1-5", () => {
  channel.send(notificationclass);
});

let classnotification6 = new cron.CronJob("00 35 15 * * 1-5", () => {
  channel.send(notificationclass);
});

let classnotification7 = new cron.CronJob("00 10 16 * * 1-5", () => {
  channel.send(notificationclass);
});

let classnotification8 = new cron.CronJob("00 45 16 * * 1-5", () => {
  channel.send(classbreak);
});

let classnotification9 = new cron.CronJob("00 00 17 * * 1-5", () => {
  channel.send(notificationclass);
});

let classnotification10 = new cron.CronJob("00 35 17 * * 1-5", () => {
  channel.send(
    `You have ended your classes for today! enjoy the rest of your day`
  );
});

// When you want to start it, use:
classnotification1.start();
classnotification2.start();
classnotification3.start();
classnotification4.start();
classnotification5.start();
classnotification6.start();
classnotification7.start();
classnotification8.start();
classnotification9.start();
classnotification10.start();
});

// login to Discord with token
client.login(token);
```
As you see the cronjob format of time is:
- Seconds: 0-59
- Minutes: 0-59
- Hours: 0-23
- Day of Month: 1-31
- Months: 0-11 (Jan-Dec)
- Day of Week: 0-6 (Sun-Sat)

So right now is where you want to edit to your school scheadule and your own messages!
Try it to be useful (like the zoom link of the class for example) so it can save you some time!
After that, we login to discord with the `client.login(token);` function.

### Create reply comments

```javascript
client.on("message", (message) => {
  if (message.content === `${prefix}help`) {
    // reply in the same channel from it was sent
    message.channel.send(help());

    // you concatenate messages with the else if block
  } else if (message.content === `${prefix}calendar`) {
  message.channel.send(`There you go! ${calendar}`);
  } else if (message.content === `${prefix}idukay`) {
  message.channel.send(`Voila! ${idukay}`);
  } else if (message.content === `${prefix}mail`) {
  message.channel.send(`Sent! ${gmail}`);
  } else if (message.content === `${prefix}day`) {
  message.channel.send(`Today's scheadule: ${days[today - 1]}`);
  } else if (message.content === `${prefix}info`) {
  message.channel.send(
    `This was made by Said Neder\nGithub: ${github}\nDiscord: ${discordid}`
   );
  }
});
```
First we use the `client.on()` function to listen to new messages to do something about it,
which in this case is to reply to the messages.

And then we use a big if/else block to control the flow of the program, there is where we decide
the messages that we are going to listen, to reply them.

### Finally running the bot!

And to see your final project use this commnad:
`node bot.js`
Since this runs in the back end, and we use node for that, and enjoy your new bot!

But it will only run if you are running the task, if you want to have it live 24/7 we have a
extra step!

### Setting up the bot live on heroku! (optional)
We will go straight to the chase my boi!

First we need to create in our file a git repo, we do that with:

`git init`

Then, we create a `Procfile` inside of our folder project with this text:
```Procfile
worker: npm start
```
And that means that the heroku server is going to execute the code in a form of a worker, not as a webpage.

And now we add everything to git:

`git add .`

And then we commit the files added:

`git commit -m "deployment"`

Now we need to create a heroku account and create a new app [over here!](https://www.heroku.com/)

After that we download the [heroku cli](https://devcenter.heroku.com/articles/heroku-cli) in our system,
when that is done we login into the cli with the command

`heroku login`
And after that we just push it to heroku!

`git push heroku main`

When that is done we go into the dashboard > resources and deactivate the web dyno by just clicking edit and switching off the slide,
and now we turn on the worker that should appear, now it should be left like this

![image example](/images/botheroku.png)

And after that you are done!
Hope that you enjoyed the guide!

