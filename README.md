# CanalBot
A small Python script ~~not well written~~ made to automate the addition of airing anime on my server, using Tsundere-Raws RSS page on Nyaa.
Originally made to work on a **linux** server, with [JellyFin](https://github.com/jellyfin/jellyfin), Plex or other Streaming service.

## Requirements :
- Python 3.7 at least (support is not assured for older versions)
- a [qBittorrent](https://github.com/qbittorrent/qBittorrent) Web UI, **hosted on the same machine as the script** (use qBittorrent-nox on servers)
- [HandBrakeCLI](https://github.com/HandBrake/HandBrake) installed on your linux distro (dpkg, not the flatpack version)
- [mkvtoolnix](https://github.com/nmaier/mkvtoolnix), to extract subtitles
- [feedparser](https://github.com/kurtmckee/feedparser), a python module
- [python-qBittorrent](https://github.com/v1k45/python-qBittorrent), another python module
​
### Install all requirements :
```
apt-get install python3
apt-get install qbittorrent-nox
apt-get install handbrake-cli
apt-get install mkvtoolnix
pip3 install feedparser
pip3 install python-qbittorrent
apt-get update && apt-get upgrade
```
## What does this script do ?
It will check for new episodes in the RSS every 7 minutes and if it finds one, it will download and encode the episode to the selected location.

(exactly 7 minutes to avoid getting tempban from DNS servers or Nyaa, you can choose between encoding or copying the file)
​

**This script was intended to work with Tsundere-Raws torrents, giving the script another RSS feed will certainly break the script.**
​

Default encoding settings : *(~500MB ouput file size, for a ~24mn 1080p episode)*
- Bitrate : 2500kbps x264
- Audio Bitrate : 512kbps (to get the highest bitrate)
- Subtitle : Hardub

*you can of course change the encoding settings, please refer to [HandBrakeCLI Documentation](https://handbrake.fr/docs/en/latest/cli/cli-options.html)*
​
## Usage guide :
- Install all dependencies.
- Start qBitrorrent Web UI if it wasn't already (with command `qbittorrent-nox -d`).
- Download the file `CanalBot.py` into a direcroty.
- Create a file named `anime_list.txt`, then include anime info, one line per anime, respecting this pattern : `rss_keyword, anime_folder_name, season_number, anime_name`, *see the [anime_list.txt example](https://github.com/YazZHh/CanalBot/blob/main/anime_list.txt)* (`anime_folder_name` can be set to whatever name you want).
  - ⚠️ For the keyword, be sure to use a keyword that will work for both the torrent name on nyaa and the video file name itself.
  - Also be aware that any space in the `anime_name` will be replaced by a dot.
- Now edit CanalBot.py and modify the first variables included in the `settings` class to match your settings, *see [Settings](#settings) below*.
- Now run the script in a screen (linux package : `screen -S canalbot`) on your linux server to avoid linux killing the process, and that's it.
- If the script crashes, just restart it, it won't re-add episodes, or re-encode episode.

## File Structure
My script will respect this file structure (with default settings plus extract_subtitles enabled):
```
target_folder/
└─ anime_name/
   └─ s1/
      ├─ anime_name.s1e01.suffix.mp4
      ├─ anime_name.s1e02.suffix.mp4
      ├─ anime_name.s1e03.suffix.mp4
      ├─ ...
      └─ subtitles/
         ├─ anime_name.s1e01.suffix.ass
         ├─ anime_name.s1e02.suffix.ass
         ├─ anime_name.s1e03.suffix.ass
         └─ ...
```
## Settings
- `delete_torrents_afterwards` if set to True will delete permanently torrent file *(the downloaded one, not the file encoded or copied)* from disk and qBittorrent Web UI.
- `auto_encode` do what its name does : if set to True, it will automatically encode the file, however if set to False, it will just copy the file to the desired location.
- `lang` is used to choose the language of subtitles that will be burned in the video (however Tsundere-Raws provides french subs).
- `extract_subtitles` if set to True the script will extract subtitles see the [file structure](#File-Structure).
- `suffix` define the end of the file. Episode name will be in this format, again, see the [file structure](#File-Structure).
- `quality` obviously the video quality of the episodes you want the script to download (1080p OR 720p).
- `handbrake_settings` are the settings that HandBrakeCLI will use when encoding the file.
- `target_directory` tells the script where does the episodes will be storen, see the [file structure](#File-Structure) one more time.
- `torrents_location` define on which directory torrents are downloaded.
- `linuxuser` specify which linux user should own the files.
- `user` is the user of the qBittorrent Web UI.
- `password` is the password of the qBittorrent Web UI.
- `webui_link` set the link to the qBittorrent Web UI.
​
## Customisation
- You can customise the encoding command if you want, *see [HandBrakeCLI Documentation](https://handbrake.fr/docs/en/latest/cli/cli-options.html)*.
- You can stop the script at any moment py pressing <kbd>CTRL</kbd> + <kbd>C</kbd>, otherwise it will run infinitely.
