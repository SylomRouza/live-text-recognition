# Live Scene Text Recognition

A demonstration of text detection and recognition employed in a pipeline for scene text recognition, built as part of my Final Year Project at Nanyang Technological University. Currently able to either run text recognition through a live camera feed at an average of 20 FPS or through a sequential list of demo images.

Text detection is currently done through a pre-trained EAST detection model that's available [here](https://www.dropbox.com/s/r2ingd0l3zt8hxs/frozen_east_text_detection.tar.gz?dl=1).

Text recognition, which was the project focus, is done through a self-trained CRNN model, through which its implementation in PyTorch is available [here](https://github.com/meijieru/crnn.pytorch). (Pre-trained model will be made available)

The CRNN model was trained over the [MJSynth Dataset](https://www.robots.ox.ac.uk/~vgg/data/text/) and fine-tuned with the COCO-Text dataset. Its performance against benchmark datasets along with their comparisons against what was reported in the paper are as tabulated below. Do note that the ICDAR13 test dataset was filtered according to constraints mentioned [here](https://github.com/meijieru/crnn.pytorch/issues/5) when used to measure the model's accuracy.

| Dataset | Accuracy | Reported Accuracy |
|--|--|--|
| ICDAR13 | 86.46% | 86.70% |
| IIIT 5k-words | 78.30% | 78.20% |

## Screenshots

<p align="center">
  <img src="https://raw.githubusercontent.com/joshenlim/live-text-recognition/master/screenshots/ss_1.png" width="300px" style="display: block; margin: 0 auto"/>
</p>

The program is also able to arrange and display the detected texts in the order which its meant to be read from the image as such. If the image contained multiple contexts of texts, the program is able to segregate them accordingly as well and not treat all of the words in the image as a single paragraph.

<p align="center">
  <img src="https://raw.githubusercontent.com/joshenlim/live-text-recognition/master/screenshots/ss_2.png" width="500px" style="display: block; margin: 0 auto"/>
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/joshenlim/live-text-recognition/master/screenshots/ss_3.png" width="500px" style="display: block; margin: 0 auto"/>
</p>

## Set up

Ensure that you have both a trained EAST detection model and a trained CRNN model available. Placed them within the `east_text_detector` and `crnn_text_recognizer` folders respectively. Be sure to update the paths to the model in `main.py`.  

Virtual environment can be set up via pipenv, add a `--skip--lock` flag to the install command if locking takes too long:

```
pipenv shell
pipenv install
```

Run the program with the following command, remove the `--live` flag if intend to run program for demo images:

`python -m main --viewWidth 720 --live`

Demo images are 10 randomly sampled images from the Street View Text Dataset [here](http://vision.ucsd.edu/~kai/svt/) 

Program was built and tested on Python 3.6.5, MacOS

## Possible Errors

- PyTorch - ImportError: Library not loaded: @rpath/libc++.1.dylib

Run the following command but change the path after `/usr/lib` to where the `_C.so` file of the torch library is located on your local machine, in this context I have it in a virtual env:

`install_name_tool -add_rpath /usr/lib ~/.local/share/virtualenvs/fyp-text-detect-demo-FnxzTDdA/lib/python3.6/site-packages/torch/_C.cpython-36m-darwin.so`
