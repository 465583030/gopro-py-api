# GoPro API for Python 

Unofficial GoPro API Library for Python - connect to HERO3/3+/4/5/+ via WiFi.

![](http://i.imgur.com/kA0Rf1b.png)


###Compatibility:

- HERO3
- HERO3+
- HERO4 (including HERO Session)
- HERO+
- HERO5

###Installation

```bash
git clone http://github.com/konradit/gopropy
cd gopropy
python setup.py install
```

###Documentation:

####HERO4/HERO5/HERO+ (gpcontrol)

These cameras use a new version of GoPro API which centers around /gp/gpControl/ url.

| Code | Explanation |
|------|-------------|
|     gpControlCommand(X,Y) | Sends a command to the camera, using GoPro constants |
|     gpControlSet(X,Y) | Sends a setting to the camera, using GoPro constants |
|     shutter(param) | Starts a video or takes a picture<br>param=constants.start or constants.stop |
|     shoot_video(X) | Shoots a video, X is the number of seconds the video will be, default 0, (infinity) |
|     take_photo(X) | Takes a photo, X is the time before the picture is taken. Default 0. |
|     video_settings(X,Y) | Changes the video settings<br><ul><li>X=Video Resolution: 4k / 2k / 1440p / 1080p / 960p / 480p</li><li>Y=Frame Rate: 240 / 120 / 100 / 60 / 30 / 24
|     mode(X,Y) | Changes the mode, X=Mode, Y=Submode (default is 0). Example: camera_mode(constants.Mode.PhotoMode, constants.Mode.SubMode.Photo.Single) |
|     getStatusRaw() | Returns the status dump of the camera in json |
|     getStatus(X,Y) | Returns the status. <br><ul><li>X = constants.Status.Status or constants.Status.Settings</li><li>Y = status id (Status/Setup/Video/Photo/MultiShot).</li><li>NOTE: This returns the status of the camera as an integer.</li></ul>|
|     infoCamera(option) | Returns camera information<br>option = model_number, model_name, firmware_version, serial_number
|     delete() | Can be: delete(last) or delete(all) |
|     deleteFile(folder,file) | Deletes a specific file |
|     hilight() | HiLights a moment in the video recording |
|     power_on() | Powers the camera on. NOTE: run this to put your H4 Session into app mode first! |
|     power_off() | Powers the camera off |
|     syncTime() | Syncs the camera time to the computer's time |
|     locate(param) | Makes the camera beep. locate(constants.Locate.Start) for start and locate(constants.Locate.Stop) for stop. |
|     getMedia() | returns the last media taken URL |
|     downloadLastMedia() | Downloads latest media taken |
|     listMedia() | Outputs a prettified JSON media list |
|     getMediaInfo(option) | Gets the media info<br>option=file/folder/size |
|     livestream(param) | Starts, restarts or stops the livefeed via UDP. |

####HERO3/HERO3+/HERO2 (auth):

These cameras use the traditional /camera/ or /bacpac/ GoPro API, which is now deprecated and replaced with gpControl for newer cameras starting with HERO 4.

| Code | Explanation |
|------|-------------|
|     sendCamera(X,Y) | Sends a command to the camera using /camera/. Use constants.Hero3Commands. |
|     sendBacpac(X,Y) | Sends a command to the camera using /bacpac/. Use constants.Hero3Commands. |
|     shutter(param) | Starts a video or takes a picture<br>param=constants.start or constants.stop |
|     shoot_video(X) | Shoots a video, X is the number of seconds the video will be, default 0, (infinity) |
|     take_photo(X) | Takes a photo, X is the time before the picture is taken. Default 0. |
|     video_settings(X,Y) | Changes the video settings<br><ul><li>X=Video Resolution: 4k / 2k / 1440p / 1080p / 960p / 480p</li><li>Y=Frame Rate: 240 / 120 / 100 / 60 / 30 / 24
|     mode(X,Y) | Changes the mode, X=Mode: constants.Hero3Commands.Mode.VideoMode/PhotoMode/BurstMode/TimeLapseMode |
|     getStatusRaw() | Returns the status dump of the camera in json |
|     infoCamera(option) | Returns camera information<br>option = model_name, firmware_version, ssid
|     delete() | Can be: delete(last) or delete(all) |
|     deleteFile(folder,file) | Deletes a specific file |
|     power_on_auth() | Powers the camera on. |
|     power_off() | Powers the camera off |
|     syncTime() | Syncs the camera time to the computer's time |
|     getMedia() | returns the last media taken URL |
|     downloadLastMedia() | Downloads latest media taken |
|     listMedia() | Outputs a prettified JSON media list |
|     getMediaInfo(option) | Gets the media info<br>option=file/folder/size |
|     livestream(param) | Starts, restarts or stops the livefeed /live/amba.m3u8 |

###Usage:

You can do a ton of stuff with this library, here is a snippet of how some of the commands can be used:

```python
from goprocam import GoProCamera
from goprocam import constants

gpCam = GoProCamera.GoPro()
```

NOTE: You can initialise with ```GoProCamera.GoPro()``` and it will detect which camera is connected and what API to use, this can be unreliable as I have only tested it with HERO4 and HERO3. If you want to connect to a specific camera use ```GoProCamera.GoPro(constants.gpcontrol)``` for HERO4/5/HERO+ and ```GoProCamera.GoPro(constants.auth)``` for HERO3/HERO3+.

---

```
gpCam.shutter(constants.start) #starts shooting or takes a photo

gpCam.mode(constants.Mode.VideoMode) #changes to video mode

print(gpCam.getStatus(constants.Status.Status,constants.Status.STATUS.Mode)) #Gets current mode status
>0

print(gpCam.infoCamera(constants.Camera.Name)) #Gets camera name
>HERO4 Black

print(gpCam.getStatus(constants.Status.Status, constants.Status.STATUS.BatteryLevel)) #Gets battery level
>3

print(gpCam.getStatus(constants.Status.Settings, constants.Setup.ORIENTATION)) #Gets orientation mode
>2

print(gpCam.getMedia()) #Latest media taken URL
>http://10.5.5.9:8080/videos/DCIM/104GOPRO/GOPR2386.JPG

print(gpCam.getMediaInfo("file")) #Latest media taken filename
>GOPR2386.JPG

gpCam.take_photo(5) #Takes a photo in 5 seconds.

gpCam.shoot_video(60) #Shoots a 60 second video

gpCam.video_settings("1080p","60") #Changes resolution to 1080p60

gpCam.gpControlSet(constants.Setup.BEEP, constants.Setup.Beep.OFF) #Disable beeps.

gpCam.gpControlSet(constants.Video.SPOT_METER, constants.Video.SpotMeter.ON) #Activates spot meter on video mode

```

