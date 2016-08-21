# UDUK™ Acoustic Sprite
Primarily use for automatic algorithmic composition.

## Structure
String / Fret Number, where 0 indicates open string.

## Format
MPEG ADTS, layer III, v1, 128 kbps, 44.1 kHz, Monaural.

## Demo
(http://uduk.org/zzaj)

"Banyak jalan yang lebih berirama"

### Javascript source example

Requirement: a running webserver.

````
<script>
  var audioCtx;
  var bufferGlobal = new Array();
  var index = 0;
  
  var AudioContext = window.AudioContext || window.webkitAudioContext || false; 
  if (AudioContext) {
    audioCtx = new AudioContext;
  }
    
  var getDataAudio = function(filename) { 
    var request = new XMLHttpRequest();
    request.open('GET', filename, true);
    request.responseType = 'arraybuffer';
    request.onload = function() {
      var audioData = request.response;
      audioCtx.decodeAudioData(audioData, function(buffer) {
        saveBuffer(buffer);
      },  
      function(e){e.err});
    }
    request.send();    
  };
  
  for (var i = 1; i <= 6; i++) {
    for (var j = 0; j <= 12; j++) {
      getDataAudio('acoustic/' + i + '/' + j + '.mp3');
    }
  }
  
  var saveBuffer = function (buffer) {
    bufferGlobal[index] = buffer;
    index++;
    if (index == 6 * 13) {
      console.log("%c DeepAcoustic v1.0", "background: #000; color: #fff");
      zhredThis();
    }
  };
  
  var playSound = function(buffer, t) {
    var sourceMain = audioCtx.createBufferSource();
    sourceMain.buffer = buffer;
    sourceMain.connect(audioCtx.destination);
    sourceMain.start(t); 
  };
  
  var zhredThis = function() {

    var startTime = audioCtx.currentTime + 0.100;
    var tempo = 120; 
  
    var sixteenNote = (60 / tempo) / 4;
    var timeSignature = sixteenNote;
  
    for (var bar = 0; bar < 180; bar++) {

      var time = startTime + bar * 16 * timeSignature;
   
      for (var i = 0; i < 16; ++i) {
        var r = Math.floor(Math.random() * (6 * 13 - 0)) + 0;
        if (i % 4 == 0)
          playSound(bufferGlobal[r], time + i * timeSignature);
      }
    
      for (var i = 2; i < 16; i+=4) {
        var r = Math.floor(Math.random() * (6 * 13 - 0)) + 0;
        playSound(bufferGlobal[r], time + i * timeSignature);
      }

      for (var i = 0; i < 16; ++i) {
        var r = Math.floor(Math.random() * (6 * 13 - 0)) + 0;
        if (i % 1 == 0)
          playSound(bufferGlobal[r], time + i * timeSignature);
      }

      for (var i = 0; i < 16; ++i) {
        var r = Math.floor(Math.random() * (6 * 13 - 0)) + 0;
        if (i % 8 == 0)
          playSound(bufferGlobal[r], time + i * timeSignature);
      }   
    
    }
  
  }
</script>
````

## LICENSE
UDUK™ Free as an AIR License
