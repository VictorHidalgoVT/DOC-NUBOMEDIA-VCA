.. _installation_guide:	     
	     
%%%%%%%%%%%%%%%%%%
Installation guide
%%%%%%%%%%%%%%%%%%

This section is divided in two parts. On the one hand, we see how to install the
NUBOMEDIA-VCA filters using the repositories of the project. On the other hand,
we see how to install these filters from the source code.

From repositories
=================

In order to install the latest stable version of the NUBOMEDIA-VCA filters in
your local environment, first of all you need an instance of Kurento Media
Server installed in your machine. To do that, use the following commands:

.. sourcecode:: console

   echo "deb http://ubuntu.kurento.org trusty kms6" | sudo tee /etc/apt/sources.list.d/kurento.list
   wget -O - http://ubuntu.kurento.org/kurento.gpg.key | sudo apt-key add -
   sudo apt-get update
   sudo apt-get install kurento-media-server-6.0

Then you are able to install the NUBOMEDIA-VCA filters. To do that, you have
type the following commands:

.. sourcecode:: console

   apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F04B5A6F
   add-apt-repository "deb http://repository.nubomedia.eu/ trusty main"
   sudo apt-get update -y

   # To install ear detector filter
   sudo apt-get install nubo-ear-detector nubo-ear-detector-dev  -y

   # To install eye detector filter
   sudo apt-get install nubo-eye-detector nubo-eye-detector-dev -y

   # To install face detector filter
   sudo apt-get install nubo-face-detector nubo-face-detector-dev -y

   # To install mouth detector filter
   sudo apt-get install nubo-mouth-detector nubo-mouth-detector-dev -y

   # To install nose detector filter
   sudo apt-get install nubo-nose-detector nubo-nose-detector-dev -y

   # To install tracker filter
   sudo apt-get install nubo-tracker nubo-tracker-dev -y

From source code
================

In order to install the NUBOMEDIA-VCA from the source code you need the
development packages for Kurento. To install these components use the following
commands:

.. sourcecode:: console

   echo "deb http://ubuntu.kurento.org trusty kms6" | sudo tee /etc/apt/sources.list.d/kurento.list
   wget -O - http://ubuntu.kurento.org/kurento.gpg.key | sudo apt-key add -
   sudo apt-get update
   sudo apt-get install kurento-media-server-6.0 kurento-media-server-6.0-dev
   sudo apt-get install kms-core-6.0 kms-core-6.0-dev
   sudo apt-get install cimg-dev
   sudo apt-get install devscripts

After that, you are able to generate the Debian packages of the NUBOMEDIA-VCA
filters from the source code. To do that, you need to clone the official
repository on `GitHub <https://github.com/nubomedia/NUBOMEDIA-VCA>`__ and the
execute the build script.

.. sourcecode:: console

   git clone https://github.com/nubomedia/NUBOMEDIA-VCA.git
   cd NUBOMEDIA-VCA
   ./build.sh

If everything goes well, a new folder called `output` will be created. Inside
this folder you can find the Debian packages for the different filters. To
install these filters, you need to run the following commands:

.. sourcecode:: console 

   cd output

   # To install ear detector filter
   sudo dpkg -i nubo-ear-detector_6.6.0~rc1_amd64.deb nubo-ear-detector-dev_6.6.0~rc1_amd64.deb

   # To install eye detector filter
   sudo dpkg -i nubo-eye-detector_6.6.0~rc1_amd64.deb nubo-eye-detector-dev_6.6.0~rc1_amd64.deb

   # To install face detector filter
   sudo dpkg -i nubo-face-detector_6.6.0~rc1_amd64.deb nubo-face-detector-dev_6.6.0~rc1_amd64.deb

   # To install mouth detector filter
   sudo dpkg -i nubo-mouth-detector_6.6.0~rc1_amd64.deb nubo-mouth-detector-dev_6.6.0~rc1_amd64.deb

   # To install nose detector filter
   sudo dpkg -i nubo-nose-detector_6.6.0~rc1_amd64.deb nubo-nose-detector-dev_6.6.0~rc1_amd64.deb

   # To install tracker filter
   sudo dpkg -i nubo-tracker_6.6.0~rc1_amd64.deb nubo-tracker-dev_6.6.0~rc1_amd64.deb
