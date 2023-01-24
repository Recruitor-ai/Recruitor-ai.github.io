---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults
permalink: /recruit1.html
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
 
 <button id="start-camera">Start Camera</button>
<video id="video" width="350" autoplay></video>
<button id="start-record">Start Recording</button>
<button id="stop-record">Stop Recording</button>
<a id="download-video" download="test.webm">Download Video</a>
<button id="restart-record">Redo Recording</button>
<script>

let camera_button = document.querySelector("#start-camera");
let video = document.querySelector("#video");
let start_button = document.querySelector("#start-record");
let restart_button = document.querySelector("#restart-record");
let stop_button = document.querySelector("#stop-record");
let download_link = document.querySelector("#download-video");

let camera_stream = null;
let media_recorder = null;
let blobs_recorded = [];
video.muted = true;
camera_button.addEventListener('click', async function() {
   	try {
    	camera_stream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true, volume: false });
    }
    catch(error) {
    	alert(error.message);
    	return;
    }

    video.srcObject = camera_stream;
    camera_button.style.display = 'none';
    video.style.display = 'block';
    start_button.style.display = 'block';
});

start_button.addEventListener('click', function() {
    media_recorder = new MediaRecorder(camera_stream, { mimeType: 'video/webm' });

    media_recorder.addEventListener('dataavailable', function(e) {
    	blobs_recorded.push(e.data);
    });

    media_recorder.addEventListener('stop', function() {
    	let video_local = URL.createObjectURL(new Blob(blobs_recorded, { type: 'video/webm' }));
    	download_link.href = video_local;
    video.srcObject = camera_stream;
    video.muted = false;
    video.volume = 1;
    video.controls = true;
    video.src = URL.createObjectURL(new Blob(blobs_recorded, { type: 'video/webm' }));
        stop_button.style.display = 'none';
        download_link.style.display = 'block';
    });

    

    media_recorder.start(1000);

    start_button.style.display = 'none';
    stop_button.style.display = 'block';
});

restart_button.addEventListener('click',  async function() {
    media_recorder = new MediaRecorder(camera_stream, { mimeType: 'video/webm' });
    
video.src = video.srcObject = null;
    media_recorder.addEventListener('dataavailable', function(e) {
    	blobs_recorded.push(e.data);
    });

    media_recorder.addEventListener('stop', function() {
    	let video_local = URL.createObjectURL(new Blob(blobs_recorded, { type: 'video/webm' }));
    	download_link.href = video_local;
    let camera_stream5 = navigator.mediaDevices.getUserMedia({ video: true, audio: true, volume: false });
    video.src = video.srcObject = null;
    video.muted = false;
    video.volume = 1;
    video.controls = true;
    video.src = URL.createObjectURL(new Blob(blobs_recorded, { type: 'video/webm' }));
        stop_button.style.display = 'none';
        download_link.style.display = 'block';
    });

    

    media_recorder.start(1000);

    start_button.style.display = 'none';
    stop_button.style.display = 'block';
});

stop_button.addEventListener('click', function() {
	media_recorder.stop(); 
});

</script>
  </div>
</div>


    </div> <!-- /container -->
  </body>
</html>
