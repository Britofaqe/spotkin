# Spotnik [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](https://makeapullrequest.com)

A python package that updates one or more of your Spotify playlists every day with a random selection of tracks from any public playlists. 

For example, I have a playlist called ["Rivers Radio"](https://open.spotify.com/playlist/1HaQfSGjNzIsiC5qOsCUcW?si=861bc59c458b4b0a) that is updated by Spotnik every day with this recipe:
    
    PLAYLIST NAME / QUANTITY OF TRACKS TO TAKE
    albumsImCheckingOut	8
    new albums	7
    nostalgia	7
    new music friday	7
    nu metal	6
    fivehundredalbums	6
    new_music_friday_picks	5
    liked_songs_picks	5
    tiktok	3
    today's top hits	3
    us viral	3
    recent weezer	2
    big on the internet	2
    etc.
    
I developed this script because I didn't like fiddling with the Spotify app all the time. I just wanted a great selection of music in one playlist every day. I've been it using it every day for a few years. It's run automatically at 2am by a Windows Task Scheduler job. It works best when you draw from many playlists, especially:

- dynamic playlists like "New Music Friday" or "Today's Top Hits" because they frequently change
- large curated playlists like Rolling Stone's "fivehundredalbums"
- a playlist generated by my new_albums script which has the latest album releases in all the genres you're interested in  

You can also ban artists, tracks, or genres. That's great for avoiding music you don't like (obviously) but also for avoiding music you don't want to hear right now even though you love it. For example, I love the Beach Boys. Spotify knows that so they keep adding them to the algorthmic playlists. But I've heard all their songs a zillion times and I don't need to hear them now.

On tour, I realized I needed a second playlist, for warming up before a show, so I added the ability to update as many playlists as you want--and to set minimun values for 'energy', etc. I recommend sticking with one or two playlists, though, otherwise you're just fiddling with the Spotify app all over again.

I find that I tweak my recipe about once a week.

## Installation

Before you can run the Spotnik script, there are some pre-requisites the script assumes.

### Spotify Developer Account

The script will need a Spotify **Client Id** and **Client Secret** to interact with Spotify's Web API.

Register for a [developer account](https://developer.spotify.com) on Spotify. After registering, create a new app. Once you create a new app, a Client Id and Client Secret will be generated. You will need these in later steps.

Additionally, the Spotnik script uses an Authorization Code Flow. Due to this, you will need to set a redirect URL for your app. To add a redirect URL, open the app's settings. Note: The Spotnik script is only intended to run locally, on your machine, so add a redirect link to `http://localhost:8080`.

### Spotify Playlist Id

The script will need the unique ID for one of your Spotify playlists. To get the ID for a playlist, in Spotify, right-click on the playlist > Share > Copy Share Link. The link will contain the playlist ID. It is the string between `playlist/` and `?si=`.

### Environment Variables

You can specify custom variables to include using a `.env` file.  Alternatively, you can set them as Environment Variables.

```
SPOTIFY_CLIENT_ID=xxx
SPOTIFY_CLIENT_SECRET=xxx
SPOTIFY_REDIRECT_URI=http://localhost:8080

#### For importing data from a CSV file (will be deprecated)
JOBS_FILE=path_to_jobs_file
ADD_LIST=path_to_add_list_csv

#### For importing data from Google Sheets (preferred but not yet implemented)
GSPREADER_GOOGLE_CLIENT_EMAIL=client_email_from_your_creds.json
GSPREADER_GOOGLE_CREDS_PATH=path_to_your_creds.json

```
### Poetry
poetry init
poetry install

### Define one or more jobs (target playlists to update along with the source playlists and how many tracks to take from each source)
By default, the jobs specified in `spotnik.data._default_jobs` are used.  You can add a custom jobs file in the `spotnik/data` folder and specify the name in your .env file usimg the `JOBS_FILE` variable.   

### Define your add list in a CSV file
Each row is a source playlist you want to pull tracks from to add to your target playlists. It will have at least 3 columns:
- Name: the name of the source playlist (for display purposes only)
- PlaylistId: the id of the source playlist
- And then as many target playlist names as you want. The value for each target playlist name should be the number of tracks to take from that playlist.  For example, if you want to take 5 tracks from the playlist "new music friday" and add them to your "Rivers Radio" playlist, you would have a line like this:

```new music friday,37i9dQZF1DX4JAvHpjipBk,7```


### Create a Virtual Environment (optional)

These steps is not necessary, but recommended for environment isolation. You could use [virtualenv](https://virtualenv.pypa.io/en/latest/installation.html) (and [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/index.html)) or just the built-in [venv](https://docs.python.org/3/library/venv.html).

### Install dependencies

Installing the package adds the dependencies specified in the setup.py file

```
pip install .
```

### Local development

The package can be installed in develop mode to allow you to make changes locally.

`pip install . develop`  

## Running

Once you have completed all the installation steps, run Spotnik script by running `py -m spotnik`.


## Contributing
Feel free to make pull requests for any changes you'd like to see.  

see [discussions](https://github.com/riverscuomo/spotnik/discussions/11) for some ideas.
