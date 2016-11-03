.. _Developer_guide:
	     
%%%%%%%%%%%%%%%
Developer guide
%%%%%%%%%%%%%%%

On this section, we will see the different APIs to use every single filter. As
it was explained on :doc:`introduction <introduction>`, the following filters
are available to use in the NUBOMEDIA platform: **face detector**,
**mouth detector**, **nose detector**, **eye detector**, **ear detector** and
**tracker filter**. From a developer point of view, these filters can be used
in two scenarios:

1. Using your own instance of Kurento Media Server with the proper filter
installed. If this is your case, please take a look to the
:doc:`installation guide <installation_guide>` for further information about
filter installation.

2. Using the NUBOMEDIA PaaS to host your application. If this is your case, you
don't need to worry about the filter installation since the platform has these
filters installed out of the box. In this case, you need to use the proper Java
dependency in your application. To add this dependency, first you need to
include the following directive in your pom.xml (this part is a must to locate
the NUBOMEDIA-VCA Maven artifacts):

.. sourcecode:: xml

   <repositories>
      <repository>
         <id>kurento-releases</id>
         <name>Kurento Repository</name>
         <url>http://maven.kurento.org/releases</url>
         <releases>
            <enabled>true</enabled>
         </releases>
         <snapshots>
            <enabled>false</enabled>
         </snapshots>
      </repository>
   </repositories>

Then, you will be able to add the proper NUBOMEDIA-VCA Maven artifact, namely:

* Face detector:

.. sourcecode:: xml

      <dependency>
         <groupId>org.kurento.module</groupId>
         <artifactId>nubofacedetector</artifactId>
         <version>6.6.0</version>
      </dependency>

* Mouth detector:

.. sourcecode:: xml

      <dependency>
         <groupId>org.kurento.module</groupId>
         <artifactId>nubomouthdetector</artifactId>
         <version>6.6.0</version>
      </dependency>

* Nose detector:

.. sourcecode:: xml

      <dependency>
         <groupId>org.kurento.module</groupId>
         <artifactId>nubonosedetector</artifactId>
         <version>6.6.0</version>
      </dependency>

* Eye detector:

.. sourcecode:: xml

      <dependency>
         <groupId>org.kurento.module</groupId>
         <artifactId>nuboeyedetector</artifactId>
         <version>6.6.0</version>
      </dependency>

* Ear detector:

.. sourcecode:: xml

      <dependency>
         <groupId>org.kurento.module</groupId>
         <artifactId>nuboeardetector</artifactId>
         <version>6.6.0</version>
      </dependency>

* Tracker filter:

.. sourcecode:: xml

      <dependency>
         <groupId>org.kurento.module</groupId>
         <artifactId>nubotracker</artifactId>
         <version>6.6.0</version>
      </dependency>

The following sections provides information of the Java API provided by each
NUBOMEDIA-VCA component.

Face, mouth, nose, eye and ear
==============================

All this filters have a similar API, for this reason, we are going to see all of
them together.

**NuboFaceDetector**

This filter receives a stream of images as input. The output of the filter will
be a collection of bounding boxes. Each bounding box represents the position of
each face in the image. A bounding box is an area defined by two points. It is
very important to highlight that this algorithm only detects front faces.
Therefore, all the faces that are laterally focused will not be detected.

**NuboMouthDetector , NuboNoseDetector, NuboEarDetector, NuboEyeDetector**

As for mouth, nose, eye and ear detector, these filters receive a stream of
images as input. The output of each filter will be a collection of bounding
boxes. Each bounding box represents the position of each mouth,nose, eye and
found in the image. These algorithms needs to detect previously the different
faces included on the image, with the exception of the ear detector which have
to detect the side of the face. The faces can be detected by its own, or can be
received as an input.

The developers can use the following API for this filter:


=================================== ===========================================================
 Function                           | Description                                                
----------------------------------- -----------------------------------------------------------
void **showX(int)** *               | To show or hide the bounding boxes of the detected faces,    
                                      mouths, ears, noses,
				    | and eyes within the image. 
                                    |  
                                    | Parameter’s value:
				    |  - 0 (default), the bounding boxes will not be shown.
				    |  - 1, the bounding boxes will be drawn in the frame
----------------------------------- -----------------------------------------------------------
void **detectByEvent(int)**         | To indicate to the algorithm if it must process all the
                                      images or only when
			            | it receives a specific event such as motion detection. 
			            | 
			            | Parameter’s value:
			            |  - 0 (default) , process all the frames;
			            |  - 1 , process a frame when it receives a specific event
----------------------------------- -----------------------------------------------------------
void **sendMetaData(int)**          | To send the bounding boxes of the faces, mouths, eyes
                                      noses and ears detected to
				    | another ME as a metadata.
			            | 
			            | Parameter’s value:
			            |  - 0 (default) , metadata are not sent
			            |  - 1 , metadata are sent
----------------------------------- -----------------------------------------------------------
void **widthToProcess(int)**        | To indicate the width of the image that the algorithm is 
                                      going to process to 
                                    | another ME as a metadata.
			            | 
			            | Parameter’s value:
			            |  - 160 (default), 240, 320, 640 
----------------------------------- -----------------------------------------------------------
void **processXevery4Frames(int)**  | To indicate the number of frames that the algorithm process
                                      every 4 frames.
			            | 
			            | Parameter’s value:
			            |  - 1, processes one image and discard 3 (8 fps)
				    |  - 2, processes two images and discard 2 (12 fps)
				    |  - 3, processes three images and discard 1 (18 fps)
				    |  - 4, processes four images  (24 fps)
=================================== ===========================================================

\* **showX** can be depending on the algorithm: showFaces(int), showNoses(int), showMouths(int), showEyes(int), showEars(int).

Tracker
=======

The developers can use the following API for this filter:

=================================== ===========================================================
 Function                           | Description                                                
----------------------------------- -----------------------------------------------------------
void **setVisualMode(int)**         | To show or hide the objects detected. 
			            |  
			            | Parameter’s value:
                                    |  - 0 (default), the bounding boxes will not be shown.
			            |  - 1, the bounding boxes will be drawn in the frame
----------------------------------- -----------------------------------------------------------
void **setThreshold(int)**          | To set up the minumum difference among pixels to 
                                       consider motion
			            | 
			            | Parameter’s value:
			            |  - 0-255 (20 default) 
----------------------------------- -----------------------------------------------------------
void **setMinArea(int)**            | To set up the minumum area to consider objects
			            | 
			            | Parameter’s value:
			            |  - 0 - 10000 (50 default) 
----------------------------------- -----------------------------------------------------------
void **setMaxArea(int)**            | To set up the maximum area to consider objects
			            | 
			            | Parameter’s value:
			            |  - 0 - 300000 (30000 default)
----------------------------------- -----------------------------------------------------------
void **setDistance(int)**           | To set up the distance among object to merge them
			            | 
			            | Parameter’s value:
			            |  - 0 - 2000 (35 Default) 
=================================== ===========================================================

