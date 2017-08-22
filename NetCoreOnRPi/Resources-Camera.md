# Start developing with .NET Core and the Raspberry PI
## Useful resources for the Raspberry PI and NetCore 2.0

## Videos
The Raspberry PI camera can record H264 video but the data is not enclosed inside an "MP4" container, it is just raw H264 video data.
For example, you can run this command to record a video and save it on a file:
```
raspivid -t 30000 -w 1920 -h 1080 -o video.h264
```
Not all players can play this raw H264 data and this apply also to the code that is acquired using libraries.
To wrap the H264 data in a "MP4" package, you should install [MP4Box](https://github.com/gpac/gpac) which include the omxplayer and some utilities.
```
sudo apt-get install -y gpac
```
This is the command to create the MP4 file:
```
MP4Box -add video.h264 video.mp4
```
And this is the command to start the player locally on the RPi:
```
omxplayer video.mp4
```
Additional info can be [found here](https://www.raspberrypi.org/documentation/usage/camera/raspicam/raspivid.md).
