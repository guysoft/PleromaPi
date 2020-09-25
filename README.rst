PleromaPi
=========

An out of the box `Raspberry Pi <http://www.raspberrypi.org/>`_ Raspbian distro that runs `_Pleroma <https://pleroma.social/>`_ using Docker and Nginx-proxy as a reverse proxy, with letsencrypt.


Where to get it?
----------------

Official mirror is `here <http://unofficialpi.org/Distros/PleromaPi>`_

How to use it?
--------------

#. Unzip the image and install it to an SD card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Configure your WiFi by editing ``pleromapi-wpa-supplicant.txt`` at the root of the flashed card when using it like a flash drive
#. Configure the Pleroma settings at ``/boot/docker-compose/pleroma/environments/pleroma/pleroma.env`` and set::

    ADMIN_EMAIL=admin@ops.pleroma.social
    NOTIFY_EMAIL=pleroma+admin@ops.pleroma.social
    DOMAIN=pleroma.gnethomelinux.com
    VIRTUAL_HOST=pleroma.gnethomelinux.com
    LETSENCRYPT_HOST=pleroma.gnethomelinux.com
    LETSENCRYPT_EMAIL=guysoft@gmail.com

#. Boot the Pi from the SD card
#. Hostname is ``pleromapi`` (not ``raspberrypi`` as usual), username: ``pi`` and inital password is: ``raspberry``
#. After a few mintues you should be able to access ``http://pleromapi.local/``
#. You can change the settings of the Pleroma/nginx-proxy stack in the files located at ``/boot/docker-compose/pleroma/`` and ``/boot/docker-compose/nginx-proxy/``.


Requirements
------------
* A domain pointing to your Pi's IP.
* 2A power supply
* RaspberryPi 2, 3B, 3B+, 4B (not 1 and zero)

Features
--------

* Pleroma Pre-installed using docker
* Nginx-proxy to manage reverse proxy and certificates


Developing
----------

Requirements
~~~~~~~~~~~~

#. Docker or Vagrant, docker recommended
#. Docker-compose - recommended if using docker build method, instructions assume you have it
#. Downloaded `Raspberry Pi OS <https://downloads.raspberrypi.org/raspios_lite_armhf/images/>`_ image.
#. Root privileges for chroot
#. Bash
#. sudo (the script itself calls it, running as root without sudo won't work)

Build PleromaPi
~~~~~~~~~~~~~~~

PleromaPi can be built using docker running either on an intel or RaspberryPi (supported ones listed).
Build requires about 4.5 GB of free space available.
You can build it assuming you already have docker and docker-compose installed issuing the following commands::

    
    git clone https://github.com/guysoft/PleromaPi.git
    cd PleromaPi/src/image
    wget -c --trust-server-names 'https://downloads.raspberrypi.org/raspios_lite_armhf_latest'
    cd ..
    sudo docker-compose up -d
    sudo docker exec -it pleromapi-build build
    
Building PleromaPi Variants
~~~~~~~~~~~~~~~~~~~~~~~~~~~

PleromaPi supports building variants, which are builds with changes from the main release build. An example and other variants are available in the folder ``src/variants/example``.

To build a variant use::

    sudo docker exec -it pleromapi-build build [Variant]
    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build PleromaPi in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

To use it::

    sudo apt-get install vagrant nfs-kernel-server
    sudo vagrant plugin install vagrant-nfs_guest
    sudo modprobe nfs
    cd PleromaPi/src/vagrant
    sudo vagrant up

After provisioning the machine, its also possible to run a nightly build which updates from devel using::

    cd PleromaPi/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd PleromaPi/src/vagrant
    run_vagrant_build.sh [Variant]

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/config``. If you need to override the path to the Raspbian image to use for building PleromaPi, override the path to be used in ``ZIP_IMG``. By default, the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. Run ``src/build_dist`` as root.
#. The final image will be created in ``src/workspace``

Code contribution would be appreciated!
