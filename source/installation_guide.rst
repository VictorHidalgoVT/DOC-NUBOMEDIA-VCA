.. _installation_guide:	     
	     
%%%%%%%%%%%%%%%%%%
Installation guide
%%%%%%%%%%%%%%%%%%

This sections is divided into parts. On the one hand, we will see how to install
the filters using the repositories of the project. On the other hand we will
see how to install the filters as both demos from source code.

Previous steps
==============

Before beginning these parts, you need to install the following packages:

.. sourcecode:: console

   echo "deb http://ubuntu.kurento.org trusty kms6" | sudo tee /etc/apt/sources.list.d/kurento.list
   wget -O - http://ubuntu.kurento.org/kurento.gpg.key | sudo apt-key add -
   sudo apt-get update
   sudo apt-get install kurento-media-server-6.0 kurento-media-server-6.0-dev
   sudo apt-get install kms-core-6.0 kms-core-6.0
   sudo apt-get install cimg-dev

Installing filters from nubomedia repositories
==============================================

In order to isntall the latest stable version of the NUBOMEDIA-vca filters, you
have type the following commands, one at time and in the same order as listed
here. When asked for any kind of confirmation, reply affirmatively:

.. sourcecode:: console

   apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F04B5A6F
   add-apt-repository "deb http://repository.nubomedia.eu/ trusty main"
   apt-get update -y
   apt-get install nubo-face-detector nubo-face-detector-dev nubo-mouth-detector nubo-mouth-detector-dev nubo-nose-detector nubo-nose-detector-dev nubo-eye-detector nubo-eye-detector-dev nubo-tracker nubo-tracker-dev nubo-ear-detector nubo-ear-detector-dev -y

Installing filters and demos from source code
=============================================

In this section, we will see how to install all modules and demos from the
source code. For this purpose, you need to clone the official repository on
`GitHub  <https://github.com/nubomedia/NUBOMEDIA-VCA>`__ .

.. sourcecode:: console

  git clone https://github.com/nubomedia/NUBOMEDIA-VCA.git nubo-vca

After cloning the repo is time to compile, follow these instructions

.. sourcecode:: console

   cd nubo-vca/
   sh build.sh

If everything goes well, a new folder with the following content has been
generated

.. sourcecode:: console 

	 output/
	 ├── apps
	 │   ├── NuboEarJava.zip
	 │   ├── NuboEyeJava.zip
	 │   ├── NuboFaceJava.zip
	 │   ├── NuboFaceProfileJava.zip
	 │   ├── NuboMouthJava.zip
	 │   ├── NuboNoseJava.zip
	 │   └── NuboTrackerJava.zip
	 └── packages
	     ├── nubo-ear-detector_0.0.4~rc1_amd64.deb
	     ├── nubo-ear-detector-dev_0.0.4~rc1_amd64.deb
	     ├── nubo-eye-detector_0.0.4~rc1_amd64.deb
	     ├── nubo-eye-detector-dev_0.0.4~rc1_amd64.deb
	     ├── nubo-face-detector_0.0.4~rc1_amd64.deb
	     ├── nubo-face-detector-dev_0.0.4~rc1_amd64.deb
	     ├── nubo-mouth-detector_0.0.4~rc1_amd64.deb
	     ├── nubo-mouth-detector-dev_0.0.4~rc1_amd64.deb
	     ├── nubo-nose-detector_0.0.4~rc1_amd64.deb
	     ├── nubo-nose-detector-dev_0.0.4~rc1_amd64.deb
	     ├── nubo-tracker_0.0.4~rc1_amd64.deb
	     └── nubo-tracker-dev_0.0.4~rc1_amd64.deb

For install the filters, you need to run the following commands:

.. sourcecode:: console 

   cd output/packages
   sudo dpkg -i nubo-ear-detector_0.0.4~rc1_amd64.deb nubo-ear-detector-dev_0.0.4~rc1_amd64.deb
   nubo-eye-detector_0.0.4~rc1_amd64.deb nubo-eye-detector-dev_0.0.4~rc1_amd64.deb
   nubo-face-detector_0.0.4~rc1_amd64.deb nubo-face-detector-dev_0.0.4~rc1_amd64.deb
   nubo-mouth-detector_0.0.4~rc1_amd64.deb nubo-mouth-detector-dev_0.0.4~rc1_amd64.deb
   nubo-nose-detector_0.0.4~rc1_amd64.deb nubo-nose-detector-dev_0.0.4~rc1_amd64.deb
   nubo-tracker_0.0.4~rc1_amd64.deb nubo-tracker-dev_0.0.4~rc1_amd64.deb
   
For install the demos, you need to run the following commands for every zip file
contained in the output/apps folder. We will make the example for the face
detector

.. sourcecode:: console 

   cd output/apps
   mkdir face
   mv NuboFaceJava.zip face/
   unzip -x NuboFaceJava.zip
   sudo sh install.sh

Run the demos
=============

To run the difference demos, you need to acces the following url's through a web
browser compliant with WebRTC.

.. sourcecode:: console 

  - localhost:8100 => Face detector
  - localhost:8102 => Nose detector
  - localhost:8103 => Mouth detector
  - localhost:8104 => Ear detector
  - localhost:8105 => Face profile
  - localhost:8107 => Tracker
  - localhost:8108 => Eye detector
