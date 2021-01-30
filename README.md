# Beta-Commands
this is a version that plays the first search result without a search result message

1. Always make a backup of your files before useing any of these commands
2. replace the play.js file in /Master-Bot/commands/music folder
3. Enjoy



this alters lines 372 through 499
```
  static async searchYoutube(query, message, voiceChannel) {
    const videos = await youtube
      .searchVideos(query, 1)
      .catch(async function () {
        await message.say(
          ":x: There was a problem searching the video you requested!"
        );
        return;
      });
    if (!videos) {
      message.say(
        `:x: I had some trouble finding what you were looking for, please try again or be more specific.`
      );
      return;
    }
    // const vidNameArr = [];
    // for (let i = 0; i < videos.length; i++) {
    //   vidNameArr.push(
    //     `${i + 1}: [${videos[i].title
    //       .replace(/&lt;/g, '<')
    //       .replace(/&gt;/g, '>')
    //       .replace(/&apos;/g, "'")
    //       .replace(/&quot;/g, '"')
    //       .replace(/&amp;/g, '&')
    //       .replace(/&#39;/g, "'")}](${videos[i].shortURL})`
    //   );
    // }
    // vidNameArr.push('cancel');
    // const embed = new MessageEmbed()
    //   .setColor('#ff0000')
    //   .setTitle(`:mag: Search Results!`)
    //   .addField(':notes: Result 1', vidNameArr[0])
    //   .setURL(videos[0].url)
    //   .addField(':notes: Result 2', vidNameArr[1])
    //   .addField(':notes: Result 3', vidNameArr[2])
    //   .addField(':notes: Result 4', vidNameArr[3])
    //   .addField(':notes: Result 5', vidNameArr[4])
    //   .setThumbnail(videos[0].thumbnails.high.url)
    //   .setFooter('Choose a song by commenting a number between 1 and 5')
    //   .addField(':x: Cancel', 'to cancel ');
    // var songEmbed = await message.channel.send({ embed });
    // message.channel
    //   .awaitMessages(
    //     function(msg) {
    //       return (
    //         (msg.content > 0 && msg.content < 6) || msg.content === 'cancel'
    //       );
    //     },
    //     {
    //       max: 1,
    //       time: 60000,
    //       errors: ['time']
    //     }
    //   )
    //   .then(function(response) {
    //     const videoIndex = parseInt(response.first().content);
    //     if (response.first().content === 'cancel') {
    //       songEmbed.delete();
    //       return;
    //     }
    youtube
      .getVideoByID(videos[0].id)
      .then(function (video) {
        // // can be uncommented if you don't want the bot to play live streams
        // if (video.raw.snippet.liveBroadcastContent === 'live') {
        //   songEmbed.delete();
        //   return message.say("I don't support live streams!");
        // }

        // // can be uncommented if you don't want the bot to play videos longer than 1 hour
        // if (video.duration.hours !== 0) {
        //   songEmbed.delete();
        //   return message.say('I cannot play videos longer than 1 hour');
        // }

        // // can be uncommented if you don't want to limit the queue
        // if (message.guild.musicData.queue.length > 10) {
        //   songEmbed.delete();
        //   return message.say(
        //     'There are too many songs in the queue already, skip or wait a bit'
        //   );
        // }
        message.guild.musicData.queue.push(
          PlayCommand.constructSongObj(video, voiceChannel, message.member.user)
        );
        if (message.guild.musicData.isPlaying == false) {
          message.guild.musicData.isPlaying = true;
          // if (songEmbed) {
          //   songEmbed.delete();
          // }
          PlayCommand.playSong(message.guild.musicData.queue, message);
        } else if (message.guild.musicData.isPlaying == true) {
          // if (songEmbed) {
          //   songEmbed.delete();
          // }
          const addedEmbed = new MessageEmbed()
            .setColor("#ff0000")
            .setTitle(`:musical_note: ${video.title}`)
            .addField(
              `Has been added to queue. `,
              `This song is #${message.guild.musicData.queue.length} in queue`
            )
            .setThumbnail(video.thumbnails.high.url)
            .setURL(video.url);
          message.say(addedEmbed);
          return;
        }
      })
      .catch(function () {
        // if (songEmbed) {
        //   songEmbed.delete();
        // }
        message.say(
          ":x: An error has occured when trying to get the video ID from youtube."
        );
        return;
      });
    // })
    // .catch(function() {
    //   if (songEmbed) {
    //     songEmbed.delete();
    //   }
    //   message.say(
    //     ':x: Please try again and enter a number between 1 and 5 or cancel.'
    //   );
    //   return;
    // });
  }
  ```
