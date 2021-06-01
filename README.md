# Beta-Commands

this is a version restricts the Say Command to only the Admins

1. Always make a backup of your files before using any of these commands
2. replace the say.js file in /Master-Bot/commands/other folder
3. Enjoy

this adds `userPermissions: ["ADMINISTRATOR"],`

```
const { Command } = require('discord.js-commando');
const { MessageEmbed } = require('discord.js');

module.exports = class SayCommand extends Command {
  constructor(client) {
    super(client, {
      name: 'say',
      aliases: ['make-me-say', 'print'],
      memberName: 'say',
      group: 'other',
      guildOnly: true,
      clientPermissions: ['SEND_MESSAGES', 'MANAGE_MESSAGES'],
      userPermissions: ['ADMINISTRATOR'],
      description: 'Make the bot say anything!',
      args: [
        {
          key: 'channel_name',
          prompt: 'In which channel do you want the announcement to be sent?',
          type: 'channel'
        },
        {
          key: 'text',
          prompt: ':microphone2: What do you want the bot to say?',
          type: 'string'
        }
      ]
    });
  }

  run(message, { channel_name, text }) {
    const embed = new MessageEmbed()
      .setTitle(`Just wanted to say...`)
      .setColor('#888888')
      .setDescription(text)
      .setTimestamp()
      .setFooter(
        `${message.member.displayName}, made me say it!`,
        message.author.displayAvatarURL()
      );

    channel_name.send(embed).catch(e => console.error(e));
    return;
  }
};
```
