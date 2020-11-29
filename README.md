# ATEM Mini Pro (ISO) RTMP Webcam
Use the RTMP stream of the ATEM Mini Pro (ISO) as webcam thus allowing to record using the USB port.
# Prerequisites
The following software is needed:
 - ATEM Software Control (https://www.blackmagicdesign.com/support/family/atem-live-production-switchers)
 - RTMP Server
	 - ffmpeg (https://ffmpeg.org/) or
	 - nginx with RTMP module (https://www.nginx.com/products/nginx/modules/rtmp-media-streaming/) or
	 - ...
 - VLC Media Player (https://www.videolan.org/)
 - NDI Tools (https://ndi.tv/tools/)
# Install the software
Install the aforementioned software and make sure to select at least these options when installing the NDI Tools:
 - Webcam Input
 - WLC Media Player Plugin
 - NDI | HX v1 Drivers
 - (optional) Studio Monitor for debugging purposes
# Add the custom streaming settings
In order to stream to a local server you need to add a custom streaming config to your ATEM. An example is given here: [LocalRtmpServer.xml](https://github.com/mb-auh/AtemMiniProRtmpWebcam/blob/main/LocalRtmpServer.xml)
Make sure to set the correct IP address.
 - Open ATEM Software Control
 - Go to "Stream", select "Load Streaming Settings" and import your prepared custom streaming settings XML
 - Click on "Output" and select your imported Platform

# Configure VLC
In order to send the video stream received by VLC to NDI you need to configure the audio and video output module:
 - Open VLC
 - Open Preferences (CTRL + P or Tools -> Preferences)
 - Select "Audio" and change the "Output module" from "Automatic" to "NDI audio output"
 - Select "Video" and change the "Output" from "Automatic" to "NDI video output"
 - Restart VLC

# Start the RTMP server
## ffmpeg
Start ffmpeg from the command line to server two RTMP servers (one for the incoming and one for the outgoing video):

    ffmpeg -f flv -listen 1 -i rtmp://<IP of your RTMP Server>:1935/live -c copy -f flv -listen 1 rtmp://<IP of your RTMP Server>:8889/live
Set the correct IP Addresses. RTMP Server is this device. Here port 1935 (default RTMP) will accept the input of the ATEM and port 8889 will serve VLC.
Make sure to allow ffmpeg to accept connections from the ATEM if using a local firewall.
**! ATTENTION !**
If either video stream (from ATEM or to VLC) crashes you will have to restart ffmpeg. This problem can be worked around by making ffmpeg and VLC restart by themselves or by using another RTMP server like nginx.
## nginx
TBD
# Go "ON AIR"
Start the stream on the ATEM. After a few seconds the "ON AIR" LED should stop blinking, showing the the stream is being processed by your local RTMP server.
# Connect VLC
Start VLC and go to "Media -> Open Network Stream" (CTRL + N). Enter the URL you set earlier, as example:

    rtmp://<IP of your RTMP Server>/live
Click Play. You will not see any video or hear any audio.
# Optional: Check your stream using Studio Monitor
You can check the video and audio stream by using the NDI Studio Monitor. Click on the hamburger menu (top left corner) and select your NDI Server and here the VLC stream.
# Start the virtual webcam
Start the NDI Tools "Virtual Webcam". Right-click the tray icon and select your NDI Server and here the VLC stream. If you do not see the virtual webcam or any video in your desired application make sure to restart it.
