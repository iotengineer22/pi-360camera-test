# pi-360camera-test
This repository present solution for my raspberry-pi 360 camera test


## Trying YOLOX with Raspberry Pi and ONNX Runtime

I tested object detection using YOLOX with ONNX on a Raspberry Pi.


### Installation

Here is what I installed after booting up the Raspberry Pi.

I made it possible to use `pip` on the Raspberry Pi by modifying the `pip.conf` file to include `break-system-packages = true`.

```bash
sudo apt-get update
sudo apt-get install -y libatlas-base-dev
sudo apt-get install -y python3-matplotlib python3-numpy python3-opencv
sudo apt-get install vim
sudo vim /etc/pip.conf
# add: break-system-packages = true
sudo pip3 install -U pip
sudo pip3 install onnxruntime
```


### Python Test Program

The Python test program is available on GitHub:

[https://github.com/iotengineer22/pi-360camera-test/blob/main/onnx-official-yolox.py](https://github.com/iotengineer22/pi-360camera-test/blob/main/onnx-official-yolox.py)


### YOLOX ONNX Model

I used the ONNX model (YOLOX-Nano) provided by the official YOLOX repository.

[https://yolox.readthedocs.io/en/latest/demo/onnx_readme.html](https://yolox.readthedocs.io/en/latest/demo/onnx_readme.html)

You can download it using the `wget` command below:

```bash
wget https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_nano.onnx
```


### Testing YOLOX with ONNX Runtime

Here is an overview of the test video content. The Raspberry Pi 4 is booted up.

We check the program. The YOLOX-nano ONNX model is already downloaded.

We see that it uses ONNX Runtime for inference on the CPU.

```python
# ***********************************************************************
# Use ONNX Runtime
# ***********************************************************************

session = onnxruntime.InferenceSession(
    'yolox_nano.onnx',
    providers=["CPUExecutionProvider"]
)
```


Next, we perform object detection on a photo (showing a burger, fries, and beer) using YOLOX.


### Running the Program

```bash
cd pi-360camera-test/
python3 onnx-official-yolox.py
```


### Test Results

The results show the detected objects, their coordinates, scores, and processing times.

```text
bboxes of detected objects: [[ 813.33984375    6.96622419 1112.49267578  326.20492554]
 [   0.           19.16614342 1234.06774902  720.        ]
 [  80.62242126  265.93841553  484.27999878  702.06433105]
 [ 192.60302734  350.97183228  332.86862183  553.54443359]
 [ 143.62518311  499.18865967  318.77157593  669.91772461]
 [ 283.67523193  333.55316162  434.64361572  397.6237793 ]
 [ 373.16641235  212.27023315  930.09777832  705.84484863]]
scores of detected objects: [0.47573426 0.41899779 0.20007721 0.14844385 0.13801597 0.12921971
 0.1230136 ]
Details of detected objects: [39. 60. 45. 46. 52. 46. 45.]
Pre-processing time: 0.0342 seconds
CPU execution time: 0.1721 seconds
Post-process time: 0.0112 seconds
Total run time: 0.2176 seconds
Performance: 4.596205391876461 FPS
```

The detected image is saved as `result.jpg` in the `img` folder in the same directory.

The object detection was successful.



## Object Detection and Live Streaming with Raspberry Pi and 360-Degree Camera

We experimented with live streaming from Python using a Raspberry Pi and a 360-degree camera. 

The setup allowed us to capture 360-degree images in real-time.

We also conducted tests for 360-degree object detection using YOLOX. The Raspberry Pi and the 360-degree camera successfully performed object detection.


### Test Environment

We used a Raspberry Pi 4 running a 64-bit OS.

The 360-degree camera used was the RICOH THETA V. It's an older model with USB 2.0 connectivity, but it has various APIs and libraries available.


### Installation

To enable live streaming from a USB camera, we need to install some libraries like GStreamer, libuvc, and v4l2loopback-dkms.

```bash
sudo apt install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-pulseaudio
sudo apt install v4l2loopback-dkms
sudo apt-get install libusb-1.0-0-dev
```

```bash
git clone https://github.com/nickel110/libuvc.git
cd libuvc/
mkdir build
cd build/
cmake ..
sudo apt-get install cmake
cmake ..
make && sudo make install
```

We also need to install the libraries required for the RICOH THETA camera.

```bash
git clone https://github.com/nickel110/gstthetauvc.git
cd gstthetauvc/thetauvc/
make
sudo cp gstthetauvc.so /usr/lib/aarch64-linux-gnu/gstreamer-1.0
sudo /sbin/ldconfig -v
```

Set the GStreamer plugin path:

```bash
echo $GST_PLUGIN_PATH
sudo vim ~/.bashrc 
# Add: export GST_PLUGIN_PATH=/usr/lib/aarch64-linux-gnu/gstreamer-1.0
source ~/.bashrc
echo $GST_PLUGIN_PATH
gst-inspect-1.0 thetauvcsrc
```

### Python Test Programs

Programs used in each test:

- Object Detection with Webcam: `app_gst-yolox-onnx-normal-camera.py`
- 360-Degree Live Streaming: `gst-test-360-2divide.py`
- 360-Degree Object Detection: `app_gst-yolox-onnx-360-camera.py`


### Pre-Test for Object Detection with Webcam (YOLOX)

Before testing with the 360-degree camera, we conducted a pre-test using a regular webcam.

The webcam used is a "Logitech C270n," which is a very affordable camera with a resolution of 640x480 at 30fps. It is sufficient for our test.

The Raspberry Pi and the webcam are connected via USB. We defined a pipeline with GStreamer to capture data, and YOLOX performs object detection on that data, displaying the results using OpenCV.


### Live Streaming with Raspberry Pi and 360-Degree Camera

We tested live streaming using a Raspberry Pi and a 360-degree camera, specifically the RICOH THETA V. 

We review the program briefly. It uses GStreamer to capture data from the 360-degree camera.



### Object Detection (YOLOX) with Raspberry Pi and 360-Degree Camera 

We tested object detection (YOLOX) using a Raspberry Pi and a 360-degree camera.

We successfully performed 360-degree object detection.



