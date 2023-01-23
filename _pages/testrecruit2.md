---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
permalink: /recruit2.html
layout: none
---
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="icon" href="/docs/4.1/assets/img/favicons/favicon.ico">

    <title>Grid Template for Bootstrap</title>

    <link rel="canonical" href="https://getbootstrap.com/docs/4.1/examples/grid/">

    <!-- Bootstrap core CSS -->
    <link href="/css/bootstrap1.min.css" rel="stylesheet">

    <!-- Custom styles for this template -->
    <link href="/css/grid.css" rel="stylesheet">
    <script src="/js/RecordRTC.js"></script>
  </head>

  <body>
    <div class="container">

    <div class="row">
  <div class="col-md-4">
  
  <video width="350" controls>
  <source src="/video/vid1.mp4" type="video/mp4">
  <source src="mov_bbb.ogg" type="video/ogg">
  Your browser does not support HTML video.
</video>

  </div>
  <div class="col-md-4 ml-auto">
 
  <video id="vid1" controls autoplay playsinline></video>
 <button id="btn-start-recording">Start Recording</button>
<button id="btn-stop-recording" disabled>Stop Recording</button>
<script src="/js/RecordRTC.js"></script>
<script>
//var video = document.querySelector('video');
var video = document.getElementById('vid1');
function captureCamera(callback) {
    navigator.mediaDevices.getUserMedia({ audio: true, video: true }).then(function(camera) {
        callback(camera);
    }).catch(function(error) {
        alert('Unable to capture your camera. Please check console logs.');
        console.error(error);
    });
}
function stopRecordingCallback() {
    video.src = video.srcObject = null;
    video.muted = false;
    video.volume = 1;
    video.src = URL.createObjectURL(recorder.getBlob());
    recorder.camera.stop();
    recorder.destroy();
    recorder = null;
}
var recorder; // globally accessible
document.getElementById('btn-start-recording').onclick = function() {
    this.disabled = true;
    captureCamera(function(camera) {
        video.muted = true;
        video.volume = 0;
        video.srcObject = camera;
        recorder = RecordRTC(camera, {
            type: 'video'
        });
        recorder.startRecording();
        // release camera on stopRecording
        recorder.camera = camera;
        document.getElementById('btn-stop-recording').disabled = false;
    });
};
document.getElementById('btn-stop-recording').onclick = function() {
    this.disabled = true;
    recorder.stopRecording(stopRecordingCallback);
};
</script>
  </div>
</div>


    </div> <!-- /container -->
  </body>
</html>
