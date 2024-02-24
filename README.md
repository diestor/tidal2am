# Tidal to Apple Music
## Based on the work of [@therealmarius](https://github.com/therealmarius/Spotify-2-AppleMusic))
Import your Tidal playlist to Apple Music **for free** using Python!

## Usage
### 1. Export your Tidal Playlist to a .CSV file
The first thing you need is to get the songs you want into a .CSV file. So, go to [tunemymusic.com/en/transfer](https://www.tunemymusic.com/es/transfer) to download any playlist into this format. Please note that this mod only works with the format provided by this website.
You just need to login and then all the playlists that you have saved in your library should appear. Then export the CSV file of the playlist you want to convert and save it in the same directory as the directory where you cloned the repository.

### 2. Match the Tidal songs with their Apple Music identifier and upload them to Apple Music
To upload your converted IDs to an Apple Music playlist, you'll need 4 things:
- The list of Apple Music identifiers (iTunes identifiers) for each song in your Tidal playlist
- Your Apple Music Authorization (Bearer Token)
- Your session cookies
- Your Apple Music Media-User-Token

Here's a step by step to get all of this data:
1. Fire up your favorite browser and open  [Apple Music web player](https://music.apple.com).
2. Open DevTools (**Ctrl + Shift + I or Cmd + Opt + I**) and go to the Network tab. 
3. Then you'll need to log in to your account. If you're already logged in, please log out and log in again. 
4. Go back to the DevTools and look for a GET request to *https://buy.music.apple.com/account/web/info* (It seems like there are 2 requests to this URL; it should be the second one).
5. In the **Requests Headers**, copy the **Authorization**, the **Cookies** and the **Media-User-Token** .
7. Now you're finally ready to connvert your songs and push them onto your Apple Music playlist. To do so, open a terminal and run the following:
```bash
python3 transfer.py yourplaylist.csv
```
or
```bash
python3 transfer.py playlistdir 
```
(Replace *yourplaylist* by your own filename (keep the extension!), the one you got from [tunemymusic.com/en/transfer](https://www.tunemymusic.com/es/transfer).
Follow the script prompt, and when asked, paste in each data. If your terminal have a paste character limit: please hardcode them OR put them into separate files named as following: `token.dat`, `cookies.dat`, and `media_user_token.dat`.



## Limitations & Notes
### iTunes StoreFront Region
Please note that the current script is configured to the Malaysia Storefront, so please update it with your current region. If you're not sure which region you are, check the last bit in the Cookies, those two letters are your region code, which you should replace in line 59 of the script.
### Missing songs
This modification has also edited the results when a song is not found. This script creates a new .csv file containing the songs it did not found, subtitled [yourname_missing.csv] so you can make a new run with the same data from the session with this new .csv so you can make sure there are no songs lost in the way.
