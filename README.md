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

### Frequent File Flushing
```
libcamera-vid --bitrate 50000000 --codec h264 --framerate 20 --height 768 --width 1080 --timeout 0 --output - | ffmpeg -i - -c:v copy -f h264 video.h264
```

## Convert .h264 to .mp4
```
ffmpeg -i video.h264 -c:v copy video.mp4
```

## Recording audio
```
#!/bin/bash

# Directory where audio files will be saved
output_dir="/home/pi/audios"
mkdir -p "$output_dir"

# Infinite loop to keep recording
while true; do
  # Generate a timestamp for each segment
  timestamp=$(date +"%Y%m%d_%H%M%S")
  output_file="${output_dir}/audio_${timestamp}.mp3"

  # Record audio in a segment (e.g., 5 minutes) with better quality settings
  ffmpeg -f alsa -i default -c:a libmp3lame -b:a 320k -ar 48000 -t 300 "$output_file"

  # Add a small delay between segments to ensure proper file closing
  sleep 1
done

```
