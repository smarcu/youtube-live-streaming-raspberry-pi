# Youtube Live Streaming setup for raspberry pi

[http://teakdata.com/blog/youtube-live-streaming-device-using-raspberry-pi/](http://teakdata.com/blog/youtube-live-streaming-device-using-raspberry-pi/)

## INSTALL libav-tools

```shell
sudo apt-get install libav-tools
```

## INSTALL FFMPEG

```shell

cd /usr/src

git clone git://git.videolan.org/x264

cd x264

./configure --host=arm-unknown-linux-gnueabi --enable-static --disable-opencl

make

sudo make install

cd /usr/src

git clone https://github.com/FFmpeg/FFmpeg.git

cd ffmpeg

sudo ./configure --arch=armel --target-os=linux --enable-gpl --enable-libx264 --enable-nonfree

make

sudo make install

```

## INSTALL sox

```shell
sudo apt install sox

sudo apt-get install libsox-fmt-mp3
```

## download mp3 sample and create loop
 //save the file (as example sample.mp3), create loop_sound.mp3 using sox

```shell
sox sound.mp3 loop_sound.mp3 repeat 1000
```

## RUN STREAM shell

```shell
raspivid -rot 90 -o - -t 0 -vf -hf -fps 30 -b 6000000 | ffmpeg -i loop_sound.mp3  -re -ar 44100 -ac 2 -acodec pcm_s16le -f s16le -ac 2 -i /dev/zero -f h264 -i - -vcodec copy -acodec aac -ab 128k -g 50 -strict experimental -shortest -f flv rtmp://a.rtmp.youtube.com/live2/YOUR_KEY
```

