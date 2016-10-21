
layout: post
title: Bloc Jams
thumbnail-path: "img/blocjams.png"
short-description: Bloc Jams is a generic music streaming web application that hosts a collection of albums and a playable list of songs.



{:.center}
![]({{ site.baseurl }}/img/blocjams.png)

## Summary

Bloc Jams is a music streaming web application that demonstrates some HTML, CSS, JavaScript and jQuery skills.

## Explanation

Bloc Jams is a demo project I built that integrates several things: responsive design and animations for effect, a functioning collection of album pages that are rendered via JavaScript and the ability to play a sample album. Originally DOM scripting was done via vanilla JavaScript, only to be refactored into jQuery later.

## Problem

As this project was used solely to learn and practice my new found front end development skills, I can't say too much for a real world client problem that was solved here. But I can comment on some of the challenges faced in developing a music streaming website.

* Creating mobile-friendly responsive design.
* Dynamically presenting album informaton (songs) with functioning effects and interface buttons in one function.
* Implementing logic in code for what to do when a user hits next/previous on the player's interface, or by manually selecting a different song instead.

## Solution

Responsive Design:
* This involved using viewport and media queries with CSS. I didn't use any frameworks for this.

Presenting Song Rows, here's the code:
{% highlight javascript %}
var createSongRow = function(songNumber, songName, songLength) {
    var template = 
     '<tr class="album-view-song-item">'
    +'  <td class="song-item-number" data-song-number="'+ songNumber +'">' + songNumber + '</td>'
    +'  <td class="song-item-title">' + songName + '</td>'
    +'  <td class="song-item-duration">' + filterTimeCode(songLength) + '</td>'
    +'</tr>'
    ;

    var $row = $(template);
    
    var clickHandler = function() {
        var songNumber = parseInt($(this).attr('data-song-number'));
        
        if (currentlyPlayingSongNumber !== null) {
            var currentlyPlayingCell = getSongNumberCell(songNumber);
            currentlyPlayingCell.html(currentlyPlayingSongNumber);
        } 
        
        if (currentlyPlayingSongNumber !== songNumber) {
            $(this).html(pauseButtonTemplate);
            setSong(songNumber);
            currentSoundFile.play();
            updateSeekBarWhileSongPlays();
            updatePlayerBarSong();
            
            var $volumeFill = $('.volume .fill');
            var $volumeThumb = $('.volume .thumb');
            $volumeFill.width(currentVolume + '%');
            $volumeThumb.css({left: currentVolume + '%'});
        } else if (currentlyPlayingSongNumber === songNumber) {
            if (currentSoundFile.isPaused()) {
                $(this).html(pauseButtonTemplate);
                $('.main-controls .play-pause').html(playerBarPauseButton);
                currentSoundFile.play();
                updateSeekBarWhileSongPlays();
            } else {
                $(this).html(playButtonTemplate);
                $('.main-controls .play-pause').html(playerBarPlayButton);
                currentSoundFile.pause();
            }
        }
    };
    
    var onHover = function(event) {
        var songNumberCell = $(this).find('.song-item-number'); 
        var songNumber = parseInt(songNumberCell.attr('data-song-number'));
        
        if (songNumber !== currentlyPlayingSongNumber) {
            songNumberCell.html(playButtonTemplate);
        }
    };
    var offHover = function(event) {
        var songNumberCell = $(this).find('.song-item-number'); 
        var songNumber = parseInt(songNumberCell.attr('data-song-number'));
        
        if (songNumber !== currentlyPlayingSongNumber) {
            songNumberCell.html(songNumber);
        }
    };
    
    $row.find('.song-item-number').click(clickHandler);
    $row.hover(onHover, offHover);
    return $row;
};
{% endhighlight %}

As you can tell, there's a few functions involved in creating song rows. Essentially, we're creating this function to pass into a function that sets the current album. Aside creating a row of songs from the album, we also embed the relevant hovering effects you get when the mouse is over the song's rows.

Here's how I tackled "next song" (I didn't include the code for previous song as it is mostly similar, with a few differences). Also, ignore the additional function references, otherwise I may end up having to include the entire file - although if you want to see the whole thing, it's available on my GitHub! Note: some of these comments within the code are how I kept myself on track. I figured I'd leave my thought process in there for your viewing pleasure.

{% highlight javascript %}
var nextSong = function() {
/*
- Know what the previous song is. This includes the situation in which the next song is the first song, following the final song in the album (that is, it should "wrap" around).
- Use the trackIndex() helper function to get the index of the current song and then increment the value of the index.
- Set a new current song to currentSongFromAlbum.
- Update the player bar to show the new song.
- Update the HTML of the previous song's .song-item-number element with a number.
- Update the HTML of the new song's .song-item-number element with a pause button.
*/
    var getLastSongNumber = function(index) {
        return index == 0 ? currentAlbum.songs.length : index;
    };
    
    var currentSongIndex = trackIndex(currentAlbum, currentSongFromAlbum);
    currentSongIndex++;
    
    if (currentSongIndex >= currentAlbum.songs.length) {
        currentSongIndex = 0;
    }
    
    //Set a new current song
    setSong(currentSongIndex + 1);
    
    currentSoundFile.play();
    updateSeekBarWhileSongPlays();    
    //Update player bar info
    //You want to show new song. You will change text of a few elements
    updatePlayerBarSong();
    
    var lastSongNumber = getLastSongNumber(currentSongIndex);
    //this will allow us to reset the pause or playbutton templates to the song's number. Otherwise it would just show us the pause button everytime we change the song.
    var $nextSongNumberCell = getSongNumberCell(currentlyPlayingSongNumber);
    var $lastSongNumberCell = getSongNumberCell(lastSongNumber);
    
    $nextSongNumberCell.html(pauseButtonTemplate);
    $lastSongNumberCell.html(lastSongNumber);
};
{% endhighlight %}

## Conclusion

In the end, I was able to execute and understand JavaScript and jQuery much better after this project. There were plenty of functions involved in Bloc Jams, and a lot of passing them around - sometimes with callback functions too.
