## iGAN: Interactive Image Generation powered by GAN
[[Project Webpage]](http://www.eecs.berkeley.edu/~junyanz/projects/gvm/) [[Youtube Video]](https://youtu.be/9c4z6YsBGQ0)  
Contact: Jun-Yan Zhu (junyanz at eecs dot berkeley dot edu)

## Overview
This is the authors' implementation of interactive image generation interface described in:  
"Generative Visual Manipulation on the Natural Image Manifold"   
[Jun-Yan Zhu](https://people.eecs.berkeley.edu/~junyanz/), [Philipp Krähenbühl](http://www.philkr.net/), [Eli Shechtman](https://research.adobe.com/person/eli-shechtman/), [Alexei A. Efros](https://people.eecs.berkeley.edu/~efros/)    
In European Conference on Computer Vision (ECCV) 2016

<img src='pics/demo_teaser.jpg' width=800>


Given a few user brushes, our system could generate photo-realistic samples that best satisfy the user edits at real-time. The interface is based on deep generative model such as Generative Adversarial Networks ([GAN](https://arxiv.org/abs/1406.2661)). The system serves two main purposes:
* An intelligent drawing interface for automatically generating images inspired by the color and shape of the digital brush strokes.
* An interactive visual debugging tool for understanding and visualizing deep generative models. By interacting with the deep generative models, a developer can understand which kinds of stuff that the model can produce or not.

We are working on supporting more generative models (e.g. VAE) under more deep learning frameworks (e.g. Tensorflow).  Please cite our paper if you find this code useful in your research.


## Getting started
* Install the python libraries. (See `Requirements`)
* Download the code from the Github.
* Download the pre-trained GAN models (See `Pre-trained models`).
* Run the code:
``` bash
THEANO_FLAGS='device=gpu0, floatX=float32, nvcc.fastmath=True' python iGAN_main.py --model_name outdoor_64 --model_type dcgan_theano
```

## Requirements
The code is written in Python2 and requires the following 3rd party libraries:
* numpy
* [OpenCV](http://opencv.org/)
```bash
sudo apt-get install libopencv-dev
```
  * As an alternative to opencv, you can install scikit-image
```bash
sudo pip install -U scikit-image
```
* [Theano](https://github.com/Theano/Theano)
```bash
sudo pip install --upgrade --no-deps git+git://github.com/Theano/Theano.git
```
* [PyQt4](https://wiki.python.org/moin/PyQt4): more details on Qt installation can be found [here](http://www.saltycrane.com/blog/2008/01/how-to-install-pyqt4-on-ubuntu-linux/)
```bash
sudo apt-cache search pyqt
sudo apt-get install python-qt4
```
* [Qdarkstyle](https://github.com/ColinDuquesnoy/QDarkStyleSheet)
```bash
sudo pip install qdarkstyle
```
* [dominate](https://github.com/Knio/dominate):
```bash
sudo pip install dominate
```
* GPU + CUDA + CUDNN:
The code is tested on GTX Titan X + CUDA 7.5 + CUDNN 5.1.  Here are the tutorials on how to install [CUDA](http://www.r-tutor.com/gpu-computing/cuda-installation/cuda7.5-ubuntu) and [CUDNN](http://askubuntu.com/questions/767269/how-can-i-install-cudnn-on-ubuntu-16-04). A decent GPU is required to run the system at near real-time. If you run the program on a gpu server, you need to use a remote desktop software (e.g. VNC).

## Pre-trained models:
Download the theano [DCGAN](https://github.com/Newmu/dcgan_code) model (e.g. outdoor_64). Before using our system ,please check out the random real images vs. DCGAN generated samples to see which kinds of images that a model can produce.

``` bash
bash ./models/scripts/download_dcgan_model.sh outdoor_64.dcgan_theano
```
* [ourdoor_64.dcgan_theano](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/models/theano_dcgan/outdoor_64.dcgan_theano) (64x64): trained on 150K landscape images from MIT [Places](http://places.csail.mit.edu/) dataset. [[Real](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/samples/outdoor_64_real.png) vs. [DCGAN](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/samples/outdoor_64_dcgan.png)]
* [church_64.dcgan_theano](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/models/theano_dcgan/church_64.dcgan_theano) (64x64): trained on 126k church images from the [LSUN](http://lsun.cs.princeton.edu/2016/) challenge. [[Real](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/samples/church_64_real.png) vs. [DCGAN](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/samples/church_64_dcgan.png)]
* [shoes_64.dcgan_theano](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/models/theano_dcgan/shoes_64.dcgan_theano) (64x64): trained on 50K shoes images collected by [Yu and Grauman](http://vision.cs.utexas.edu/projects/finegrained/utzap50k/). [[Real](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/samples/shoes_64_real.png) vs. [DCGAN](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/samples/shoes_64_dcgan.png)]
* [handbag_64.dcgan_theano](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/models/theano_dcgan/handbag_64.dcgan_theano) (64x64): trained on 137K handbag images downloaded from Amazon. [[Real](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/samples/handbag_64_real.png) vs. [DCGAN](https://people.eecs.berkeley.edu/~junyanz/projects/gvm/samples/handbag_64_dcgan.png)]

We also provide a script to generate sample images from the dcgan model.
```bash
THEANO_FLAGS='device=gpu0, floatX=float32, nvcc.fastmath=True' python generate_samples.py --model_name outdoor_64 --output_image outdoor_64_dcgan.png
```


## Interface:  
See [[Youtube Video]](https://youtu.be/9c4z6YsBGQ0?t=2m18s) at 2:18s for the interactive image generation demos.  
<img src='pics/ui_intro.jpg' width=800>  

#### Layout
* Drawing Pad: This is the main window of our interface. A user can apply different edits via our brush tools and the system will display the generated image. Check/Uncheck `Edits` button to display/hide user edits.  
* Candidate Results: a display showing thumbnails of all the candidate results (e.g. different modes) that fits the user edits. A user can click a mode (highlighted by a green rectangle), and the drawing pad will show this result.
* Brush Tools:  `Coloring Brush` for changing the color of a specific region; `Sketching brush` for outlining the shape. `Warping brush` for modifying the shape more explicitly.
* Slider Bar: drag the slider bar to explore the interpolation sequence between the initial result (i.e. random generated image) and the current result (e.g. image that satisfies the user edits).
* Control Panel: `Play`: play the interpolation sequence; `Fix`: fix  `Restart`: restart the system; `Save`: save the result to a folder. `Edits`: Check the box if you would like to show edits on top of generated image.


#### User interaction
* Coloring Brush:  right click to select the color; hold left click to paint; scroll the mouse wheel to adjust the width of the brush.
* Sketching Brush: hold left click to sketch the shape.
* Warping Brush: We recommend you first use coloring and sketching before the warping tools. Right click to select the a square region; hold left click to drag the region; scroll the mouse wheel to adjust the size of the square region.
* Shortcuts: P for `Play`, F for `Fix`, R for `Restart`; S for `Save`; E for `Edits`; Q for quiting  the program

## Command line arguments:
Type `python iGAN_main.py --help` for a complete list of the arguments. Here we discuss some critical arguments:  
* `--model_name`: the name of the model (e.g. outdoor_64, shoes_64, etc.)
* `--model_type`: currently only supports dcgan_theano.
* `--model_file`: the file that stores the generative model; If not specified, `model_file='./models/%s.%s' % (model_name, model_type)`
* `--top_k`: the number of the candidate results being displayed


## Plugging in you own data and models
* DCGAN_theano model on new datasets: we will provide a model training script soon (by Sep 25 2016). The script can train a model (e.g. cat.dcgan_theano) given a new photo collection. (e.g. cat_photos/)
* New deepgenerative models based on Theano (e.g. VAE：variational autoencoder): The current design of our package follows: ui python class (e.g. `ui_draw.py`) => constrained optimization python class (`constrained_opt_theano.py`) => deep generative model python class (e.g. `dcgan_theano.py`). To incorporate your own generative model, you need to create a new python class (e.g. `vae_theano.py`) under `model_def` folder with the same interface of `dcgan_theano.py`, and specify `--model_type vae_theano` in the command line.
* Generative models based on another deep learning framework (e.g. Tensorflow): we are working on a tensorflow based optimization class (e.g. `constrained_opt_tensorflow.py`) now. Once the code is released, you can create your own tensorflow model class (e.g. `dcgan_tensorflow.py`.)

## TODO
* Support Python3
* Provide our dcgan model training scripts.  
* Support Tensorflow GAN models.
* Support other deep generative models (e.g. variational autoencoder)
* Support sketch models for sketching guidance, inspired by [ShadowDraw](http://vision.cs.utexas.edu/projects/shadowdraw/shadowdraw.html).
* Support the average image mode for visual data exploration, inspired by [AverageExplorer](https://people.eecs.berkeley.edu/~junyanz/projects/averageExplorer/).


## Citation
```
@inproceedings{zhu2016generative,
  title={Generative Visual Manipulation on the Natural Image Manifold},
  author={Zhu, Jun-Yan and Kr{\"a}henb{\"u}hl, Philipp and Shechtman, Eli and Efros, Alexei A.},
  booktitle={Proceedings of European Conference on Computer Vision (ECCV)},
  year={2016}
}
```

## Cat Paper Collection
If you love cats, and love reading cool vision and graphics papers, please check out Cat Paper Collection:  
[[Github]](https://github.com/junyanz/CatPapers) [[Webpage]](http://people.eecs.berkeley.edu/~junyanz/cat/cat_papers.html)
## Acknowledgement
* We modified the DCGAN [code](https://github.com/Newmu/dcgan_code) in our package. Thanks the authors for sharing the code. Please cite the original [DCGAN](https://arxiv.org/abs/1511.06434) paper if you use their models.
* This work was supported, in part, by funding from Adobe, eBay and Intel, as well as a hardware grant from NVIDIA. J.-Y. Zhu is supported by Facebook Graduate Fellowship.
