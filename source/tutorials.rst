.. _tutorials:

%%%%%%%%%
Tutorials
%%%%%%%%%

This section contains tutorials showing how to use the different VCA filters.
These tutorials can be found within the
`GitHub NUBOMEDIA-vca repository <https://github.com/nubomedia/NUBOMEDIA-VCA>`__.
By default, this demos uses a local Kurento Media Server with the proper filter
installed. The available tutorials are the following:

- :doc:`Ear detector <ear_detector>`. This web application consists on a
  WebRTC video communication with a **ear detector filter**.

- :doc:`Eye detector <eye_detector>`. This web application consists on a
  WebRTC video communication with a **eye detector filter**.

- :doc:`Face detector <face_detector>`. This web application  consists on a
  WebRTC video communication with a **face detector filter**.

- :doc:`Face profile <face_profile>`. This web application consists on a
  pipeline composed by a WebRTC video communication with
  **face, mouth, nose and eye detector filter**

- :doc:`Mouth detector <mouth_detector>`. This web application consists on a
  WebRTC video communication with a **mouth detector filter**.

- :doc:`Nose detector <nose_detector>`. This web application consists on a
  WebRTC video communication with a **nose detector filter**.

- :doc:`Tracker <tracker>`. This web application  consists on a WebRTC video
  communication with a **tracker detector filter**.

In order to execute these demos you need to execute these commands:

.. sourcecode:: console

   git clone https://github.com/nubomedia/NUBOMEDIA-VCA.git

   # To run ear detector demo
   cd NUBOMEDIA-VCA/apps/NuboEarJava
   mvn spring-boot:run

   # To run eye detector demo
   cd NUBOMEDIA-VCA/apps/NuboEyeJava
   mvn spring-boot:run

   # To run face detector demo
   cd NUBOMEDIA-VCA/apps/NuboFaceJava
   mvn spring-boot:run

   # To run face profile demo
   cd NUBOMEDIA-VCA/apps/NuboFaceProfileJava
   mvn spring-boot:run

   # To run mouth detector demo
   cd NUBOMEDIA-VCA/apps/NuboMouthJava
   mvn spring-boot:run

   # To run nose detector demo
   cd NUBOMEDIA-VCA/apps/NuboNoseJava
   mvn spring-boot:run

   # To run tracker demo
   cd NUBOMEDIA-VCA/apps/NuboTrackerJava
   mvn spring-boot:run

In order to get the implementation details for each tutorial please visit the
following pages:

.. toctree:: :maxdepth: 1

   face_detector.rst
   nose_detector.rst
   mouth_detector.rst
   eye_detector.rst
   ear_detector.rst
   face_profile.rst
   face_profile.rst
   tracker.rst
