# useful-commands

## Stream video and audio from Pi to Youtube
```
libcamera-vid --bitrate 50000000 --codec h264 --framerate 20 --height 768 --width 1080 --timeout 0 -o - | ffmpeg -re -ar 44100 -ac 2 -acodec pcm_s16le -f s16le -ac 2 -i /dev/zero \
-f h264 -i - -vcodec copy -acodec aac -ab 128k -g 50 -strict experimental -f flv -flvflags no_duration_filesize -preset veryfast "rtmps://x.rtmps.youtube.com:443/live2/xxx-xxx-xxx"
```

## Recording video
```
libcamera-vid --bitrate 50000000 --codec h264 --framerate 20 --height 768 --width 1080 --timeout 0 -o video.h264
```

## Convert .h264 to .mp4
```
ffmpeg -i video.h264 -c:v copy video.mp4
```
