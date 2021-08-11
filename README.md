This repo hold the docker files for the base images used to build many of the media machine's images.
They rebuild on a weekly basis to make sure any changes to the parent image are being picked up, as well as any OS package updates.

There are 3 different tags available:

* alpine_latest -> holds the latest alpine image
* ubuntu_latest -> holds the latest ubuntu 20.04 image
* latest -> defaults to the alpine image 