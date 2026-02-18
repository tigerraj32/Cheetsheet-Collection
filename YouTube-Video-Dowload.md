# Download YouTube videos:  `pytube`

Youtube video can be downloaded easily via '`pytube` python library. `pytube` is a lightweight, dependency-free
python library command line utility to download youtube videos. To use this method it is required 
python installed on your machine

## Installation

### Install pip (python package manager)

    $ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
    
    $ python get-pip.py
    
### Install pytube

    $ sudo pip install pytube
    
    $ pip list | grep pytube -- to check if pytube installed or not.
    
    
 ### Download 
 
`pytube` ship with tiny cli interface for downloadig  and probing videos.

To check the list of available video download option from youtube video link 

    $ pytube https://www.youtube.com/watch?v=HhD29dmSgNE --list
    
    
 ```
 Loading video...
<Stream: itag="18" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.42001E" acodec="mp4a.40.2" progressive="True" type="video">
<Stream: itag="22" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.64001F" acodec="mp4a.40.2" progressive="True" type="video">
<Stream: itag="137" mime_type="video/mp4" res="1080p" fps="30fps" vcodec="avc1.640028" progressive="False" type="video">
<Stream: itag="248" mime_type="video/webm" res="1080p" fps="30fps" vcodec="vp9" progressive="False" type="video">
<Stream: itag="136" mime_type="video/mp4" res="720p" fps="30fps" vcodec="avc1.4d400d" progressive="False" type="video">
<Stream: itag="247" mime_type="video/webm" res="720p" fps="30fps" vcodec="vp9" progressive="False" type="video">
<Stream: itag="135" mime_type="video/mp4" res="480p" fps="30fps" vcodec="avc1.4d4014" progressive="False" type="video">
<Stream: itag="244" mime_type="video/webm" res="480p" fps="30fps" vcodec="vp9" progressive="False" type="video">
<Stream: itag="134" mime_type="video/mp4" res="360p" fps="30fps" vcodec="avc1.4d401e" progressive="False" type="video">
<Stream: itag="243" mime_type="video/webm" res="360p" fps="30fps" vcodec="vp9" progressive="False" type="video">
<Stream: itag="133" mime_type="video/mp4" res="240p" fps="30fps" vcodec="avc1.4d400c" progressive="False" type="video">
<Stream: itag="242" mime_type="video/webm" res="240p" fps="30fps" vcodec="vp9" progressive="False" type="video">
<Stream: itag="160" mime_type="video/mp4" res="144p" fps="30fps" vcodec="avc1.4d400b" progressive="False" type="video">
<Stream: itag="278" mime_type="video/webm" res="144p" fps="30fps" vcodec="vp9" progressive="False" type="video">
<Stream: itag="140" mime_type="audio/mp4" abr="128kbps" acodec="mp4a.40.2" progressive="False" type="audio">
<Stream: itag="251" mime_type="audio/webm" abr="160kbps" acodec="opus" progressive="False" type="audio">
 ```
 

Now to download the video 

    $ pytube https://www.youtube.com/watch?v=HhD29dmSgNE --itag=248
    
    
You may noticed that some streams have both video codec and audio codec, while other have just video or just audio codec. 
This is a result of YouTube supporting a streaming technique called Dynamic Adaptive Streaming over HTTP (DASH).  

To download both audio and video  and merge them to produce desired output will need `ffmpefg` 

        $ brew install ffmpeg
        
        $ pip install ffmpeg-python -- python wrapper of ggmpeg
        
        -- finally  
        $ pytube https://www.youtube.com/watch?v=BFZCUD7qL24  -f
        
    
# Download YouTube videos:  `vlc player`

- open vlc player
- File > Open Network
- Paste youtube video link 
- Vlc will now start playig the video 
- Then window > Media Information
- At the bottom of there is `loacation` that include video url that can be downloaded
- 

# yt-dlp Usage Guide

yt-dlp is a command-line program to download videos from YouTube and other similar sites.

## Installation

Ensure you have Homebrew installed on your Mac.

```bash

brew install yt-dlp
Basic Usage
```

Download a Single Video
```bash

yt-dlp <video_url>
Replace <video_url> with the URL of the video you want to download.
```

Download an Entire Playlist
```bash

yt-dlp <playlist_url>
Replace <playlist_url> with the URL of the playlist you want to download.
```

Additional Options

Download video in best quality:

```bash

yt-dlp -f best <video_url>
```

Download only the audio:

```bash

yt-dlp -x --audio-format mp3 <video_url>
```

Specify output filename:

```bash

yt-dlp -o '~/Downloads/%(title)s.%(ext)s' <video_url>
```


