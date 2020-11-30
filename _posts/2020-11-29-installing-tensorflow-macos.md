---
toc: true
layout: post
description: Installing tensorflow_macos
categories: [tensorflow, Mac OS]
title: Installation of Tensorflow for Mac OS
---
# Background

The official installation instructions are in the Git Repo for TensorFlow for Mac:

[Mac-optimized TensorFlow and TensorFlow Addons](https://github.com/apple/tensorflow_macos)

There are two key requirements:

* macOS 11.0 Big Sur. However as you will see later the `pip` wheels that have the packages for TF Mac actually won't work if you leave the 11.0 version reference. They have to be changed to 10.6. This was discovered by the community.

* Python 3.8.  Supposedly available from the Xcode command line tools. While I installed this, I never figured out how to use them, so I just used a standard `conda` environment and chose a `conda` Python 3.8 distribution.

The instructions in this post are somewhat different than what is recommended in the repo above. The Apple Git repo suggests using a virtual environment and provides a short sell script to try this out. I could not get this to work, so I created a different approach that I have used elsewhere. Besides I don't use `virtualenv`, but rather `conda` so the instructions provided here are for installing it inside a `conda` environment and it is a very manual process at that.

The Apple developers have stated that they will look into creating a more formal approach to using `conda`, so eventually it is likely that these instructions will become outdated.


# Conda installation Instructions

While Tensorflow for MacOS is supposed to be able to work on both Intel and the new M1 based Macs, these installation instructions have only been tested in an Intel based Mac. In fact as written here, they will not work because I am using the `x86`pip wheels. The ARM wheels are different.

These are the steps:

1. Create a Conda environment with Python 3.8:

	```
	conda create -n tfmac python=3.8
	```
	
2. Activate the environment: 

	```
	conda activate tfmac
	```

3. Go to 
	`https://github.com/apple/tensorflow_macos/releases/` and download from the **Assets** section towards bottom of the page: `tensorflow_macos-0.1alpha0.tar.gz`
	
4. Extract it to a folder of  your choosing.
5. In the extracted folder go to the `x86_64` folder. Inside that folder there will be a few `"*whl"` files. Their names will be in the format of: 
	
	`tensorflow_macos-0.1a0-cp38-cp38-macosx_11_0_x86_64.whl`
	
	All of the files will have the `"11_0"` string in their name. This will need to be changed. This change in the file name is (apparently) a hack that the community has come up with - without it, it will not work. 
	
	In any case, the `"11_0"` string in the name of all the "wheels" has to be changed to `"10_16"`.  For example from:
	
	`tensorflow_macos-0.1a0-cp38-cp38-macosx_11_0_x86_64.whl` 
	
	to

	`tensorflow_macos-0.1a0-cp38-cp38-macosx_10_16_x86_64.whl`
	
	Do this for all of the wheels.
	
6. Now install them with `pip`, for example:

	```
	pip install grpcio-1.33.2-cp38-cp38-macosx_10_16_x86_64.whl
	```

	Repeat for all of the wheels in the directory.
	
7. Install other packages tht are needed:
	```
    conda install -c conda-forge -y absl-py
    		conda install -c conda-forge -y astunparse
		conda install -c conda-forge -y gast
		conda install -c conda-forge -y opt_einsum
		conda install -c conda-forge -y termcolor
		conda install -c conda-forge -y typing_extensions
		conda install -c conda-forge -y wheel
		conda install -c conda-forge -y typeguard
	
	pip install tensorboard
	
	pip install wrapt flatbuffers tensorflow_estimator google_pasta keras_preprocessing protobu
	```
	
8. This will give you a basic TF for Mac environment.  You will still probably need to add other packages to the environment (if they were not added by the wheels):

* `Scikit-learn`

* `Matplotlib`

* `Etc.`


# References

1. [Apple Official repo for Tensorflow for Mac](https://github.com/apple/tensorflow_macos)

2. [Issue #3 in the Official Repo](https://github.com/apple/tensorflow_macos/issues/3). Note that the closest solution is towards the bottom.  In the end I did not use exactly this, but I combined the instructions here with the reference below.

3. [Comment from user nehbit in this issue](https://github.com/apple/tensorflow_macos/issues/7#issuecomment-730266180)
	

