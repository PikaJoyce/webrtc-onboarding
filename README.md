# WebRTC Onboarding App
-----------------------
This is an app I made to learn more about WebRTC.

### What I learned
------------------
#### getUserMedia
To begin video chatting with another user, we first have to get our own local media. To do this, we have to call `getUserMedia();`. The browser then requests permission from the user to access their camera if it is the first time the camera has been requested. If access was granted, a `MediaStream` is returned to the user which we can be used by the `<video>` element by setting the stream as the `srcObject`.

#### RTCPeerConnection
RTCPeerConnection represents a connection between a local device to a remote peer. It is an API to allow for WebRTC calls to stream audio, video, and even data! In order to set up the call between peers, three essential steps are involved:
- Creating the RTCPeerConnection and adding the local stream for both peers.
- Getting and sharing network information
  - Potential connections are called ICE Candidates
- Last but not least, getting and sharing local and remote descriptions.

We can begin by creating an RTCPeerConnection, we can pass in a `servers` argument explained below. In this application, we are using a server to help mimic real life network scenarios. We use the ICE framework to find candidates.

We then call `getUserMedia();` and pass the stream to the peerConnection. Once network candidates are available, the `onicecandidate` handler is called. We can then send serialized candidate data to a remote peer.

Once the remote peer receives a candidate message, the remote peer will add the candidate to the remote peer description by calling `addIceCandidate();`.

Peers then need to exchange local and remote audio and video information. This happens by exchanging blobs of metadata known as an `offer` and an `answer` in the SDP format. In our example, our local peer will call `RTCPeerConnection.createOffer();`. The local peer is returned a RTCSessionDescription.

If this is successful, we then send this description to the remote peer. Once the remote peer has received a description, we call `RTCPeerConnection.setRemoteDescription();`. The remote peer then responds by creating an answer by calling `RTCPeerConnection.createAnswer();`. This promise passes our remote peers' local description and is set as the remote peer's local description. This is then passed back to our original local peer.

Once our local peer receives the remote description, we have established a RTCPeerConnection!

#### STUN and TURN servers
--------------------------
In this application, I specify a STUN server. WebRTC apps can use the ICE framework in order to overcome the challenges of real life networking scenarios. ICE tries to find the best path to connect the peers.

A high level explanation of the steps that ICE takes to establish a connection:

1. ICE first tries to establish a connection using the host address gathered from a device's OS and network card.
2. If that fails, ICE obtains an external address using a STUN server.
3. If even that fails, ICE then routes traffic through a TURN relay server.

Simply put,
- A STUN server is used to get an external network address.
- A TURN server is used to relay traffic if peer to peer connection fails.

### Run the app
---------------
You can run this application locally simply by open up the `index.html` page on your browser or it can be hosted locally with these simple steps!

- Make sure you have Python on your machine:
  - To check if Python is installed, run `python --version`
  - Otherwise, install Python
- Open up your command line and run `python -m SimpleHTTP 3000`
- Now your app can be opened on `localhost:3000`
- Additionally, you can invite others to join you by using ngrok
