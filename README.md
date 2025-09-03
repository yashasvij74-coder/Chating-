# Chating-
My chating aap
const localVideo = document.getElementById('localVideo');
const remoteVideo = document.getElementById('remoteVideo');
let peerConnection;

function startCall() {
  const configuration = { 'iceServers': [{ 'urls': 'stun:stun.example.com' }] };
  peerConnection = new RTCPeerConnection(configuration);

  peerConnection.onicecandidate = function(event) {
    if (event.candidate) {
      signalingServer.send(JSON.stringify({ 'candidate': event.candidate }));
    }
  };
  peerConnection.ontrack = function(event) {
    remoteVideo.srcObject = event.streams;
  };
  navigator.mediaDevices.getUserMedia({ video: true, audio: true })
    .then(stream => {
      localVideo.srcObject = stream;
      stream.getTracks().forEach(track => peerConnection.addTrack(track, stream));
    });
}
// Set up signaling server interactions…
// More code for offer/answer/candidate handling…
