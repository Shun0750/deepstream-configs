# Config files for dewarping 360 degree video
## Spec
- Jetson Nano
- Ubuntu 18.04
- DeepStream SDK 4.0
- GStreamer 1.14.5
- 360 degree camera: Insta360 air

## What files to do
- config_dewarper*.txt: There are four config files for dewarping two fish eyes 360 degree video. You can divide four views from each directions.
- dstest1_pgie_config.txt: A config file for object recognition. It was taken from DeepStreamSDK's sample.

## How to use these files
```
gst-launch-1.0 v4l2src device=/dev/video0 ! video/x-h264,width=1920,height=960,framerate=30/1 ! nvv4l2decoder ! nvvideoconvert ! tee name=t t.src_0 ! nvdewarper config-file='./config_dewarper1.txt' ! m.sink_0 t.src_1 ! nvdewarper config-file='./config_dewarper2.txt' ! m.sink_1 t.src_2 !  nvdewarper config-file='./config_dewarper3.txt' ! m.sink_2 t.src_3 !  nvdewarper config-file='./config_dewarper4.txt' ! m.sink_3 nvstreammux name=m width=960 height=240 batch-size=8 num-surfaces-per-frame=1 ! nvmultistreamtiler columns=4 rows=1 width=960 height=240 ! nvvideoconvert ! dsexample processing-width=960 processing-height=240 full-frame=1 ! nvinfer config-file-path=./dstest1_pgie_config.txt ! queue ! nvdsosd ! nvvideoconvert ! nveglglessink window-width=960 window-height=240 sync=false
```
This command will show the window like below:

![image](https://user-images.githubusercontent.com/1614777/78845523-bf5a8700-7a43-11ea-962b-ae2c963463ad.png)
