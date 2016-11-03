.. _APIs:	     
	     
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
Reference documentation of the APIs
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

On this section, we will see the different APIs to use every single filter. As
it was explained on :doc:`What is NUBOMEDIA-VCA <what_is_nubomedia-vca>`, the
following filters are available to use in the NUBOMEDIA platform:

- Face Detector
- Mouth Detector
- Nose Detector
- Eye Detector
- Ear Detector
- Tracker

Now, let's see the API for the different filters.

Face, Mouth, Nose, Eye and Ear Detector
=======================================

All this filters have a similiar API, for this reason, we are going to see all
of them together.

**NuboFaceDetector**

This filter receives a stream of images as input. The output of the filter will
be a collection of bounding boxes. Each bounding box represents the position of
each face in the image. A bounding box is an area defined by two points. It is
very important to highlight that this algorithm only detects frontal faces.
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

