# pi-360camera-test
This repository present solution for my raspberry-pi 360 camera test

Here's the English translation for your GitHub ReadMe:

---

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

