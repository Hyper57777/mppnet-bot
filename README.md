# mppnet-bot
This module is a MPP client with token authentication. Made by defg :3
# Usage
First you need get token for the bot on official MPPNet discord server. Link: https://discord.com/338D2xMufC
When you get the token you need create file ".env" in your project folder.
If you created, then write this on the ".env" file:
```env
TOKEN=your_bot_token
```
And start create your bot:
```js
// index.js file

require('dotenv').config(); // install "dotenv" package (npm i dotenv)

const Client = require('mppnet-bot');
const client = new Client({
  ws: 'wss://mppclone.com', // uri
  token: process.env.TOKEN, // your bot token
  name: "Bot", // your bot name,
  color: "#000000", // your bot color,
  room: "Bot room", // room
  initNoteQuota: true, // if you want init note quota in your bot then - true, else - false
  initCustomTag: true, // if you want init custom tag in your bot then - true, else - false
  initPlayer: true // if you want init player for play midi files in your bot then - true, else - false
});
client.startMPP(); // for bot join MPP

client.on("hi", msg => {
  // lets set some settings for room
  
  client.chSet({
    limit: 99,
    color: "#ff0000",
    color2: "#ffffff"
  });
  
  // lets change bot's tag (for you see the tag you need install https://greasyfork.org/en/scripts/455137-mpp-custom-tags)
  // (if you dont init this, then this dont work)
  client.tag.text = "bot";
  client.tag.color = "#000000";
  
  // lets see bot's quota (if you dont init this, then this dont work)
  console.log(client.noteQuota.points)
})

// lets make some commands
client.on("a", msg => {
  const args = msg.a.split(' ');
  const cmd = args[0].toLowerCase();
  const input = msg.a.substr(cmd.length + 1);

  if (cmd == "/help") {
    client.sendChat("Commands: /ban, /unban, /crown, /dropcrown, /movecursor, /id.");
    client.sendChat("Comming soon: /play, /midis, /stop.");
  }

  if (cmd == "/ban" && msg.p._id == "your id in MPP") {
    if (!args[1] || !args[2]) return client.sendChat("Usage: /ban [_id] [min].");

    client.kickBan(args[2] * 60000, args[1]);
  }

  if (cmd == "/unban" && msg.p._id == "your id in MPP") {
    if (!args[1]) return client.sendChat("Usage: /unban [_id].");
    
    client.unBan(msg.p._id);
  }
  
  if (cmd == "/crown" && msg.p._id == "your id in MPP") {
    client.giveCrown(msg.p._id);
  }
  
  if (cmd == "/dropcrown" && msg.p._id == "your id in MPP") {
    client.dropCrown();
  }
  
  if (cmd == "/movecursor" && msg.p._id == "your id in MPP") {
    if (!args[1] || !args[2]) return client.sendChat("Usage: /movecursor [x] [y].");
    
    client.moveCursor(args[1], args[2]);
  }
  
  if (cmd == "/id") {
    client.sendChat(msg.p._id);
  }
});
```

  
