# Learn
## Please check the README.md
## Extracting songs from a Spotify playlist
### Exportify
I kept the same way that [@simonschellaert](https://github.com/simonschellaert/Exportify) used to export Spotify playlists: **Exportify**. It was developed by [@watsonbox](https://github.com/watsonbox) and it exports Spotify playlists to a CSV file with a lot of information about each song.
## Converting the CSV file to a list of iTunes identifiers
### The CSV file
Since we just got our whole playlist exported to a CSV file, I first needed to check and understand what data was available to me. After a quick lookup, I decided that I'd be using 3 data from the CSV file: **the title of the song, the artist who performed the song, and the album name**. But I also saw an interesting colon called "*ISRC*". After a quick search on Google, I found out that *ISRC* stands for International Standard Recording Code. It's a unique identifier for a song. It's a good way to identify a song, especially since music labels are encouraged to put the same across all platforms. So, it would have been the best way to match songs between Spotify and Apple Music. But unfortunately, I was not able to search for an *ISRC* in the Apple Music catalog (more on that later). So I decided to use the **title, artist, and album name** to match the songs, even though we all know that they're often not the same between music platforms.
### [The iTunes Search API](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/iTuneSearchAPI/index.html)
Since I had all the data for each song to make a search and find the best match, I needed a way to access the Apple Music catalog. The best way to do so, is to use the Apple Music API, but unfortunately, you need to have a paid, registered Apple Developer Account to use it. So I decided to keep using what was already available to me: the [iTunes Search API](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/iTuneSearchAPI/index.html). It's a public API, so you don't need to have a paid, registered Apple Developer Account to use it. But it's not as complete as the [Apple Music API](https://developer.apple.com/documentation/applemusicapi). You don't get all the same information about a song, and you can't search for a song using it ISRC (that's why I forgot them). But it's still a good way to match songs between Spotify and Apple Music. So I decided to use it. I had to change the URL since the other one was outdated, and I also changed some hearders. Hopefully, the docs for the [iTunes Search API](https://developer.apple.com/library/archive/documentation/AudioVideo/Conceptual/iTuneSearchAPI/index.html) are still available and are pretty good! Now I was able to look up the catalog and get the precious iTunes identifiers for each song. But, I still needed to make an algorithm that would test the "search term combination" and give me the best match. And that's what I did. Using multiples combination such as:
- Title + Artist + Album
- Title + Artist
- Title + Album
- Title

Then I also tryed to handle different names between music services with my algorithm that searches to see if the title is contained in each other's title, just as with artist and album names.
### The final list
The last thing that I needed to do was extract the **iTunes identifier or Apple Music identifier** (it's the same). Hopefully, the response is in JSON format, so it was really easy to extract. Now that I have all the iTunes identifiers for each song, I just need to put them in a list and export this list to a CSV file. I also added all songs that weren't matching an iTunes identifier to a *noresult.txt* file. And that's it! I had my list of iTunes identifiers for each song in my Spotify playlist ready to be uploaded to my Apple Music playlist.
## Adding songs to an Apple Music playlist
To push my songs to my Apple Music playlist, I thought about multiple different methods.
### [The Apple Music API](https://developer.apple.com/documentation/applemusicapi)
The first one and, of course, the easiest one would have been the [Apple Music API](https://developer.apple.com/documentation/applemusicapi). But as I said earlier, you need to have a paid, registered Apple Developer Account to use it. So I decided to keep looking for other methods.
### [pyautogui](https://pyautogui.readthedocs.io/en/latest/)
I first published my repo with [pyautogui](https://pyautogui.readthedocs.io/en/latest/). It's a really powerful library that lets you control your GUI, or Graphical User Interface, with Python. So, it includes your mouse and your keyboard. With all of that, my method consisted of opening the [Apple Music Web Player](https://music.apple.com) and then automating the requests with my GUI... But, I won't lie, it sucked! It was "not good", you know what I mean. However, it was working but was blocking you from using your computer while it was running. So I decided to keep looking for other methods.
### [Playwright](https://playwright.dev/)
At this point, I tried to look up the request part of the [Apple Music Web Player](https://music.apple.com). And I noticed that, obviously, you need to be logged in to make the request to push songs to a playlist. So I decided to use [Playwright](https://playwright.dev/). It's a library that lets you control a browser with Python. So I used it to login to my Apple Music account using the graphical interface of the browser and then push songs to my playlist with the request part. The problem was that I couldn't get the request part to work. I tried to use the same request as the one that the Apple Music Web Player was using, but it didn't work. It kept telling me that I wasn't logged in, even though I was. So I decided to keep looking for other methods.
### Requests and Tokens
After all of this, I realized that when you are logged in, you need to store something, maybe a token, to prove when you are making requests that you are logged in. And so I decided to analyze more the requests, and I found out that I needed 3 things to prove that I was logged in:
- The Authorization Bearer Token
- The media-user-token
- The cookies of the session

So I decided to make the requests using my Python script like the Web Player did it while also sending these 3 things within the request header, and I got approved. So to manage the authentication process, I decided that users would have to log in to their Apple Music account using the Web Player and then copy the 3 things I just talked about and paste them in the script. The last thing I needed was the playlist's unique identifier. It's in the URL of the playlist; they just have to copy and paste it.
And that's it! I was able to push songs to my Apple Music playlist using a Python script.
## Based on the work of [@simonschellaert](https://github.com/simonschellaert/spotify2am)
