<p align="left">
<img width=15% src="https://dai.lids.mit.edu/wp-content/uploads/2018/06/Logo_DAI_highres.png" alt=“SteganoGAN” />
<i>An open source project from Data to AI Lab at MIT.</i>
</p>

[![PyPI Shield](https://img.shields.io/pypi/v/steganogan.svg)](https://pypi.python.org/pypi/steganogan)
[![Travis CI Shield](https://travis-ci.org/DAI-Lab/SteganoGAN.svg?branch=master)](https://travis-ci.org/DAI-Lab/SteganoGAN)

# SteganoGAN

- License: MIT
- Documentation: https://DAI-Lab.github.io/SteganoGAN
- Homepage: https://github.com/DAI-Lab/SteganoGAN

## Overview

**SteganoGAN** is a tool for creating steganographic images using adversarial training.

## Installation

To get started with **SteganoGAN**, we recommend using `pip`:

```bash
pip install steganogan
```

Alternatively, you can clone the repository and install it from source by running `make install`:

```bash
git clone git@github.com:DAI-Lab/SteganoGAN.git
cd SteganoGAN
make install
```

For development, you can use the `make install-develop` command instead in order to install all
the required dependencies for testing and linting.

## Usage
### Command Line

**SteganoGAN** includes a simple command line interface for encoding and decoding steganographic images.

#### Hide a message inside an image

To create a steganographic image, you simply need to supply the path to the cover image and the secret 
message:

```
steganogan encode [options] path/to/cover/image.png "Message to hide"
```

#### Read a message from an image

To recover the secret message from a steganographic image, you simply supply the path to the steganographic 
image that was generated by a compatible model:

```
steganogan decode [options] path/to/generated/image.png
```

#### Additional options

The script has some additional options to control its behavior:

* `-o, --output PATH`: Path where the generated image will be stored. Defaults to `output.png`.
* `-a, --architecture ARCH`: Architecture to use, basic or dense. Defaults to dense.
* `-v, --verbose`: Be verbose.
* `--cpu`: force CPU usage even if CUDA is available. This might be needed if there is a GPU
  available in the system but the VRAM amount is too low.

**WARNING**: Make sure to use the same architecture specification (`--architecture`) during both 
the encoding and decoding stage; otherwise, `SteganoGAN` will fail to decode the message.

### Python

The primary way to interact with **SteganoGAN** from Python is through the `steganogan.SteganoGAN` 
class. This class can be instantiated using a pretrained model:

```
>>> from steganogan import SteganoGAN
>>> steganogan = SteganoGAN.load('steganogan/pretrained/dense.steg')
Using CUDA device
```

Once we have loaded our model, we can give it the input image path, the output image path, and 
the secret message:

```
>>> steganogan.encode('research/input.png', 'research/output.png', 'This is a super secret message!')
Encoding completed.
```

This will generate an `output.png` image that closely resembles the input image but contains the 
secret message. In order to recover the message, we can simply pass `output.png` to the `decode` 
method:

```
>>> steganogan.decode('research/output.png')
'This is a super secret message!'
```

## Research

We provide example scripts in the `research` folder which demonstrate how you can train your own
`SteganoGAN` models from scratch on arbitrary datasets. In addition, we provide a convenience 
script in `research/data` for downloading two popular image datasets.
