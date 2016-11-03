.. _face_profile:	     
	     
%%%%%%%%%%%%
Face Profile
%%%%%%%%%%%%

This web application  consists on a pipeline composed by a WebRTC video
communication with an face, mouth, nose and eye detector filter (loopback, the
media stream going from client to the media server and back to client).

Compiling & Running
===================

This section explain how to compile and run this tutorial in a local
environment. To install the necessary software please see the
:doc:`installation guide <installation_guide>`. To compile and run this
tutorial from the source code you can use the following commands:

.. sourcecode:: console

   git clone https://github.com/nubomedia/NUBOMEDIA-VCA.git
   cd NUBOMEDIA-VCA/apps/NuboFaceProfileJava
   mvn spring-boot:run

At this point we can test the application accessing the following URL:
`https://localhost:8443/ <https://localhost:8443/>`_.

Understanding this example
==========================
The following figure shows a screenshoot of this demo running.

.. image:: images/face_profile.png
   :alt:    face profile application
   :align:  center
   :width:  640

The interface of the application (an HTML web page) is composed by two HTML5
video tags: one showing the local stream (as captured by the device webcam) and
the other showing the remote stream sent by the media server back to the
client. The video camera stream is sent to Kurento Media Server, which
processes and sends it back to the client as a remote stream. To implement
this, we need to create a Media Pipeline composed by the following Media
Elements:

.. image:: images/FaceProfile.jpg
   :alt:    Face Profile pipeline
   :align:  center
   :width:  800

This is a web application, and therefore it follows a client-server
architecture. At the client-side, the logic is implemented in JavaScript. At
the server-side we use a Java EE application server consuming a  Client API to
control the  Media Server capabilities. To communicate these entities, two
WebSockets are used. First, a WebSocket is created between client and
application server to implement a custom signaling protocol. Second, another
WebSocket is used to perform the communication between the Java Client and the
Media Server. To communicate the client with the Java EE application server the
platform uses a simple signaling protocol based on JSON messages over
WebSocket‘s. SDP and ICE candidates needs to be exchanged between client and
server to establish the WebRtc session. If you are interested on knowing more
about the messages exchanged between them, have a look to this
`example <http://www.kurento.org/docs/current/tutorials/java/tutorial-2-magicmirror.html>`__
.

Application Server Side
=======================

This demo has been developed using a Java EE application server based on the
Spring Boot framework. This technology can be used to embed the Tomcat web
server in the application and thus simplify the development process.

In the following figure you can see a class diagram of the server side code:


.. image:: images/FaceProfileClass.png
   :alt:    face profile class diagram
   :align:  center
   :width:  480

The main class of this demo is named NuboFaceProfileJavaApp. As you can see, the
NuboMediaClient is instantiated in this class as a Spring Bean. This bean is
used to create  Media Pipelines, which are used to add media capabilities to
your applications. In this instantiation we see that we need to specify to the
client library the location of the Kurento Media Server. In this example, we
assume it’s located at localhost listening in port 8888. If you reproduce this
tutorial you’ll need to insert the specific location of your Kurento Media
Server instance there.

.. sourcecode:: java 

	@Configuration
	@EnableWebSocket
	@EnableAutoConfiguration
	public class NuboFaceProfileJavaApp implements WebSocketConfigurer {

	 final static String DEFAULT_KMS_WS_URI = "ws://localhost:8888/kurento";

	 @Bean
	 public NuboFaceProfileJavaHandler handler() {
	  return new NuboFaceProfileJavaHandler();
	 }

	 @Bean
	 public KurentoClient kurentoClient() {
	  return KurentoClient.create(System.getProperty("kms.ws.uri",
	   DEFAULT_KMS_WS_URI));
	 }

	 @Override
	 public void registerWebSocketHandlers(WebSocketHandlerRegistry registry) {
	  registry.addHandler(handler(), "/nubofaceprofiledetector");
	 }

	 public static void main(String[] args) throws Exception {
	  new SpringApplication(NuboFaceProfileJavaApp.class).run(args);
	 }
	}


This web application follows Single Page Application architecture and uses a
WebSocket to communicate client with application server by means of requests
and responses. Specifically, the main app class implements the interface
WebSocketConfigurer to register a WebSocketHanlder to process WebSocket
requests in the path /nubofaceprofiledetector.

NuboFaceProfileJavaHandler class implements TextWebSocketHandler to handle text
WebSocket requests. The central piece of this class is the method
handleTextMessage. This method implements the actions for requests, returning
responses through the WebSocket. In other words, it implements the server part
of the signaling protocol depicted.

In the designed protocol there are three different kinds of incoming messages to
the Server: start, show_faces, show_mouths, show_noses, show_eyes,
scale_factor, process_num_frames, face_resolution, mouth_resolution,
nose_resolution, eye_resolution, width_to_process,  stop and onIceCandidates.
These messages are treated in the switch clause, taking the proper steps in
each case.

.. sourcecode:: java
   
	public class NuboFaceProfileJavaHandler extends TextWebSocketHandler {

	 @Override
	 public void handleTextMessage(WebSocketSession session, TextMessage message)
	 throws Exception {
	  JsonObject jsonMessage = gson.fromJson(message.getPayload(),
	   JsonObject.class);

	  log.debug("Incoming message: {}", jsonMessage);
	  switch (jsonMessage.get("id").getAsString()) {
	   case "start":
	    start(session, jsonMessage);
	    break;
	   case "show_faces":
	    setViewFaces(session, jsonMessage);
	    break;

	   case "show_mouths":
	    setViewMouths(session, jsonMessage);
	    break;

	   case "show_noses":
	    setViewNoses(session, jsonMessage);
	    break;

	   case "show_eyes":
	    setViewEyes(session, jsonMessage);
	    break;

	   case "face_res":
	    changeResolution(FACE_FILTER, session, jsonMessage);
	    break;

	   case "mouth_res":
	    changeResolution(this.MOUTH_FILTER, session, jsonMessage);
	    break;

	   case "nose_res":
	    changeResolution(this.NOSE_FILTER, session, jsonMessage);
	    break;

	   case "eye_res":
	    changeResolution(this.EYE_FILTER, session, jsonMessage);
	    break;

	   case "fps":
	    setFps(session, jsonMessage);
	    break;

	   case "scale_factor":
	    setScaleFactor(session, jsonMessage);
	    break;

	   case "stop":
	    {
	     UserSession user = users.remove(session.getId());
	     if (user != null) {
	      user.release();
	     }
	     break;
	    }
	   case "onIceCandidate":
	    {
	     JsonObject candidate = jsonMessage.get("candidate")
	     .getAsJsonObject();
	     UserSession user = users.get(session.getId());
	     if (user != null) {
	      IceCandidate cand = new IceCandidate(candidate.get("candidate")
	       .getAsString(), candidate.get("sdpMid").getAsString(),
	       candidate.get("sdpMLineIndex").getAsInt());
	      user.addCandidate(cand);
	     }
	     break;
	    }

	   default:
	    System.out.println("Invalid message with id " + jsonMessage.get("id").getAsString());
	    sendError(session,
	     "Invalid message with id " + jsonMessage.get("id").getAsString());
	    break;
	  }
	 }
	 private void start(WebSocketSession session, JsonObject jsonMessage) {
	  ...
	 }

	 private void sendError(WebSocketSession session, String message) {
	   ...
	  }
	  ...
	}

In the following snippet, we can see the start method. It handles the ICE
candidates gathering, creates a Media Pipeline, creates the Media Elements and
make the connections among them. A startResponse message is sent back to the
client  with the SDP answer.

.. sourcecode:: java

	private void start(final WebSocketSession session, JsonObject jsonMessage) {
	  try {
	   // Media Logic (Media Pipeline and Elements)
	   UserSession user = new UserSession();
	   pipeline = kurento.createMediaPipeline();
	   user.setMediaPipeline(pipeline);
	   webRtcEndpoint = new WebRtcEndpoint.Builder(pipeline).build();
	   user.setWebRtcEndpoint(webRtcEndpoint);
	   users.put(session.getId(), user);

	   webRtcEndpoint.addOnIceCandidateListener(new EventListener < OnIceCandidateEvent > () {

	     @Override
	     public void onEvent(OnIceCandidateEvent event) {
	      JsonObject response = new JsonObject();
	      response.addProperty("id", "iceCandidate");
	      response.add("candidate", JsonUtils.toJsonObject(event.getCandidate()));
	      try {
	       synchronized(session) {
		session.sendMessage(new TextMessage(response.toString()));
	       }
	      } catch (IOException e) {
	       log.debug(e.getMessage());
	      }

	     });

	    pipeline.setLatencyStats(true); face = new NuboFaceDetector.Builder(pipeline).build(); face.sendMetaData(1); face.detectByEvent(0); face.showFaces(0);


	    mouth = new NuboMouthDetector.Builder(pipeline).build(); mouth.sendMetaData(0); mouth.detectByEvent(1); mouth.showMouths(0);

	    nose = new NuboNoseDetector.Builder(pipeline).build(); nose.sendMetaData(0); nose.detectByEvent(1); nose.showNoses(0);

	    eye = new NuboEyeDetector.Builder(pipeline).build(); eye.sendMetaData(0); eye.detectByEvent(1); eye.showEyes(0);

	    webRtcEndpoint.connect(face); face.connect(mouth); mouth.connect(nose); nose.connect(eye); eye.connect(webRtcEndpoint);

	    // SDP negotiation (offer and answer)
	    String sdpOffer = jsonMessage.get("sdpOffer").getAsString(); String sdpAnswer = webRtcEndpoint.processOffer(sdpOffer);

	    // Sending response back to client
	    JsonObject response = new JsonObject(); response.addProperty("id", "startResponse"); response.addProperty("sdpAnswer", sdpAnswer);

	    synchronized(session) {
	     session.sendMessage(new TextMessage(response.toString()));
	    }
	    webRtcEndpoint.gatherCandidates();

	   } catch (Throwable t) {
	    sendError(session, t.getMessage());
	   }
	  }
	}

The sendError method is quite simple: it sends an error message to the client
when an exception is caught in the server-side.

.. sourcecode:: java

	private void sendError(WebSocketSession session, String message) {
	 try {
	  JsonObject response = new JsonObject();
	  response.addProperty("id", "error");
	  response.addProperty("message", message);
	  session.sendMessage(new TextMessage(response.toString()));
	 } catch (IOException e) {
	  log.error("Exception sending message", e);
	 }
	}

Application Client Side
=======================

Let’s move now to the client-side of the application. To call the previously
created WebSocket service in the server-side, we use the JavaScript class
WebSocket. We use an specific JavaScript library called kurento-utils.js to
simplify the WebRTC interaction with the server. This library depends on
adapter.js, which is a JavaScript WebRTC utility maintained by Google that
abstracts away browser differences. Finally jquery.js is also needed in this
application.

These libraries are linked in the index.html web page, and are used in the
index.js. In the following snippet we can see the creation of the WebSocket
(variable ws) in the path /nubofaceprofiledetector. Then, the onmessage
listener of the WebSocket is used to implement the JSON signaling protocol in
the client-side. Notice that there are three incoming messages to client:
startResponse, error, and iceCandidate. Convenient actions are taken to
implement each step in the communication. For example, in functions start the
function WebRtcPeer.WebRtcPeerSendrecv of kurento-utils.js is used to start a
WebRTC communication.

.. sourcecode:: javascript

	var ws = new WebSocket('ws://' + location.host + '/nubofaceprofiledetector');

	ws.onmessage = function(message) {
	 var parsedMessage = JSON.parse(message.data);
	 console.info('Received message: ' + message.data);

	 switch (parsedMessage.id) {
	  case 'startResponse':
	   startResponse(parsedMessage);
	   break;

	  case 'iceCandidate':
	   webRtcPeer.addIceCandidate(parsedMessage.candidate, function(error) {
	    if (!error) return;
	    console.error("Error adding candidate: " + error);
	   });
	   break;

	  case 'error':
	   if (state == I_AM_STARTING) {
	    setState(I_CAN_START);
	   }
	   onError("Error message from server: " + parsedMessage.message);
	   break;
	  default:
	   if (state == I_AM_STARTING) {
	    setState(I_CAN_START);
	   }
	   onError('Unrecognized message', parsedMessage);
	 }
	}

	function start() {
	 console.log("Starting video call ...")
	  // Disable start button
	 setState(I_AM_STARTING);
	 showSpinner(videoInput, videoOutput);

	 console.log("Creating WebRtcPeer and generating local sdp offer ...");
	 var options = {
	  localVideo: videoInput,
	  remoteVideo: videoOutput,
	  onicecandidate: onIceCandidate
	 }

	 webRtcPeer = new kurentoUtils.WebRtcPeer.WebRtcPeerSendrecv(options,
	  function(error) {
	   if (error) {
	    return console.error(error);
	   }
	   webRtcPeer.generateOffer(onOffer);
	  });
	}

	function onOffer(error, offerSdp) {
	 if (error) return console.error("Error generating the offer");
	 console.info('Invoking SDP offer callback function ' + location.host);
	 var message = {
	  id: 'start',
	  sdpOffer: offerSdp
	 }
	 sendMessage(message);
	}


	function onIceCandidate(candidate) {
	 console.log("Local candidate" + JSON.stringify(candidate));

	 var message = {
	  id: 'onIceCandidate',
	  candidate: candidate
	 };
	 sendMessage(message);
	}

Dependencies
============

This Java Spring application is implemented using Maven. The relevant part of
the pom.xml is where NUBOMEDIA dependencies are declared.  we need  two
dependencies:  the Client Java dependency (kurento-client) and the JavaScript
Kurento utility library (kurento-utils) for the client-side. Browser
dependencies (i.e. *bootstrap*, *ekko-lightbox*, and *adapter.js*) are handled
with `Webjars <http://www.webjars.org/>`_.

.. sourcecode:: xml 

   <dependencies> 
      <dependency>
         <groupId>org.kurento</groupId>
         <artifactId>kurento-client</artifactId>
      </dependency> 
      <dependency> 
         <groupId>org.kurento</groupId>
         <artifactId>kurento-utils-js</artifactId>
      </dependency> 
   </dependencies>

.. note::

   We are in active development. You can find the latest version of
   Kurento Java Client at `Maven Central <http://search.maven.org/#search%7Cga%7C1%7Ckurento-client>`_.
