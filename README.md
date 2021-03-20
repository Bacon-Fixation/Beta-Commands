# Beta-Commands

this is a version that plays the first search result without a search result message

1. Always make a backup of your files before using any of these commands
2. replace the play.js file in /Master-Bot/commands/music folder
3. Enjoy

this alters lines 375 through 510

```
    // static createVideosResultsEmbed(namesArray, firstVideo) {
  //   return new MessageEmbed()
  //     .setColor('#ff0000')
  //     .setTitle(`:mag: Search Results!`)
  //     .addField(':notes: Result 1', namesArray[0])
  //     .setURL(firstVideo.url)
  //     .addField(':notes: Result 2', namesArray[1])
  //     .addField(':notes: Result 3', namesArray[2])
  //     .addField(':notes: Result 4', namesArray[3])
  //     .addField(':notes: Result 5', namesArray[4])
  //     .setThumbnail(firstVideo.thumbnails.high.url)
  //     .setFooter('Choose a song by commenting a number between 1 and 5')
  //     .addField(':x: Cancel', 'to cancel ');
  // }

  static async searchYoutube(query, message, voiceChannel) {
    const videos = await youtube.searchVideos(query, 1).catch(async function() {
      await message.reply(
        ':x: There was a problem searching the video you requested!'
      );
      return;
    });
    if (!videos) {
      message.reply(
        `:x: I had some trouble finding what you were looking for, please try again or be more specific.`
      );
      return;
    }
    // if (videos.length < 5) {
    //   message.reply(
    //     `:x: I had some trouble finding what you were looking for, please try again or be more specific.`
    //   );
    //   return;
    // }
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
    // const embed = PlayCommand.createVideosResultsEmbed(vidNameArr, videos[0]);
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
      .then(function(video) {
        // if (
        //   video.raw.snippet.liveBroadcastContent === 'live' &&
        //   !playLiveStreams
        // ) {
        //   songEmbed.delete();
        //   message.reply(
        //     'Live streams are disabled in this server! Contact the owner'
        //   );
        //   return;
        // }

        // if (video.duration.hours !== 0 && !playVideosLongerThan1Hour) {
        //   songEmbed.delete();
        //   message.reply(
        //     'Videos longer than 1 hour are disabled in this server! Contact the owner'
        //   );
        //   return;
        // }

        // if (message.guild.musicData.queue.length > maxQueueLength) {
        //   songEmbed.delete();
        //   message.reply(
        //     `The queue hit its limit of ${maxQueueLength}, please wait a bit before attempting to add more songs`
        //   );
        //   return;
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
          // Added song to queue message (search)
          PlayCommand.createResponse(message)
            .addField('Added to Queue', `:new: [${video.title}](${video.url})`)
            .build();
          return;
        }
      })
      .catch(function() {
        // if (songEmbed) {
        //   songEmbed.delete();
        // }
        message.reply(
          ':x: An error has occurred when trying to get the video ID from youtube.'
        );
        return;
      });
    // })
    // .catch(function() {
    //   if (songEmbed) {
    //     songEmbed.delete();
    //   }
    //   message.reply(
    //     ':x: Please try again and enter a number between 1 and 5 or cancel.'
    //   );
    //   return;
    // });
```
