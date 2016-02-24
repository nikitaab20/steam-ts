# a Steambot for TeamSpeak
[![Dependency Status](https://david-dm.org/nikitavondel/steam-ts.svg)](https://david-dm.org/nikitavondel/steam-ts)
### version

1.1.2a

```sh
$ npm install steam-ts
```

[steam-ts] is a [Node.js] module which allows for fast and easy verification between a [TeamSpeak] account and a [Steam] account of the same user.

  - Extremely easy to configure
  - No experience in programming is required in order to set this bot up if you follow the documentation carefully
  - Secure
  - Stores the **links** in a .json file
  - Allows you to set a minimum Steam level required in order to use automatic verification.
  - [MIT-license]

### Explanation

As soon as your app is running with steam-ts your Steam bot will automatically log into Steam and into your TeamSpeak query server and start listening to **!verify** commands in the Steam chat.
When someone writes **!verify** to the Steambot, it will prompt the user to give their TeamSpeak username which they are currently recognized by on the given TeamSpeak server and it will also warn them that they have to be connected to the TeamSpeak server during the process.
When the given username was correct it will poke the TeamSpeak client under that username with a randomly generated string and it will tell the user to send that string to the Steambot through the Steam chat. When the bot has successfully compared the both strings it will write to a file called **verified.json**.
This json file contains an array called users wherein each object represents a verified user.

An example of the **verified.json** file:
```json
{
    "users": [
        {
            "76561198034364892": "I7ubU2YcawFqYbzftZD3RIm8Fu4="
        },
        {
            "76561198034364892": "QuFmp68SUNzEvdNbU+6uYOHhcUQ="
        }
    ]
}
```
The keys represent the steam64id which can easily be converted to let's say a steamid, whereas the values store the TeamSpeak identity of the user which is used to distuingish users from eachother.

Another feature of this module is allowing **dynamic TeamSpeak channel names**. It allows you to select a certain supported game server and then assign it to a channel *(or channels)*, from then on it will update the name of the channel depending on the server name and the amount of players on the server. It is possible to disable this feature in the config file.

### Usage

First make sure that you've installed the module, after that we can write an extremely small piece of code which instantly sets everything up for you. All you need to do is execute a single function and everything will be up and running:

```javascript
var steamts = require('steam-ts');

steamts.launch();
//voila, your bot is now up and running!
```

But first it is **required** to adjust the **config.json**.

information about all the values inside the config.json:

```javascript
{
  "main": {
    "ts_ip": "", // The IP adress of your TeamSpeak server
    "q_username": "", // The query username of your TeamSpeak Query server (As admin: tools>ServerQuery Login)
    "q_password": "", // The query password of your TeamSpeak Query server
    "bot_username": "", // The username of your Steam bot which you use to log in.
    "bot_password": "", // The password of your Steam bot account.
    "minlevel": 0, // The minimum required Steam level of the client who wants to utilize the verification system.
    "defaultrankid": 1, // The id of the rank which users start with. (unverified rank)
    "wantedrankid": 2 // The id of the rank the bot will promote them to once they are verified. (verified rank)
  },
  "serverchannel": {
    "enabled": false, // This is a beta feature, either enable or disable it.
    "querytime": 0, // How many times (in ms) should it query the given game servers. (Do not set it lower than 10000)
    "channels":[ // An array possibly containing multiple game servers it needs to query.
      {
        "channelid": [], // An array containing the TeamSpeak channelID's which need to be manipulated (Read notes for info)
        "serverip": "", // The ip adress of the server you want to query (preferably no domain names here)
        "servertype": "", // Check the notes for more information on this one.
        "customport": 0 // Leave this at 0 if you haven't assigned a custom port to your server
      }
    ]
  }
}
```

**A few important notices:**
  - The serverchannel feature is still in a beta stage, please do forward all bugs to [this repo].
  - Channelids are obtainable by installing the [extended-default] TeamSpeak skin, otherwise make use of your TeamSpeak server query.
  - The querytime really shouldn't be lower than 10000ms (10 seconds), unless you'd like to get blocked out by your own game server.
  - Do **NOT** add the same game server twice in the channels array, instead use the channelid array to manipulate multiple TeamSpeak channels with the same information of the same server.
  - All server types can be found at [gameDig's page].

An example of the config.json file:

```javascript
{
  "main": {
    "ts_ip": "clwo.eu",
    "q_username": "nikitavondel",
    "q_password": "12345",
    "bot_username": "mybot",
    "bot_password": "54321",
    "minlevel": 5,
    "defaultrankid": 33,
    "wantedrankid": 34
  },
  "serverchannel": {
    "enabled": true,
    "querytime": 60000,
    "channels":[
      {
        "channelid": [14,19],
        "serverip": "37.59.11.113",
        "servertype": "csgo",
        "customport": 0
      },
      {
        "channelid": [1,8],
        "serverip": "37.59.11.114",
        "servertype": "csgo",
        "customport": 0
      }
    ]
  }
}
```

### Changelog
- **UPDATE 1.1.0**:
- Moved main module file into new lib directory
- Moved verified.json inside data directory
- Data directory will now be created when running the module for the first time
- Launching the bot now requires the execution of the launch() function
- Launch parameters are replaced with the config.json file
- Added more dependencies
- Added the dynamic server channel function
- For detailed information on the newly added stuff see README.md

### Development

Currently still under heavy development, please do report all the bugs you encounter.

Suggestions are extremely welcome!

If you'd like to improve this project feel free to start a pull request, it will be reviewed as fast as possible.


[steam-ts]: <https://www.npmjs.com/package/steam-ts>
[Node.js]: <https://nodejs.org>
[TeamSpeak]: <https://teamspeak.com/>
[Steam]: <https://steamcommunity.com/>
[MIT-license]: <https://opensource.org/licenses/MIT>
[extended-default]: <http://addons.teamspeak.com/directory/skins/stylesheets/Extended-Client-Info.html>
[gameDig's page]: <https://github.com/sonicsnes/node-gamedig#supported>
[this repo]: <https://github.com/nikitavondel/steam-ts>
