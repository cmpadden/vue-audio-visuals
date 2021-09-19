<template>
  <div>
    <canvas :id="_uid" :width="canvasWidth" :height="canvasHeight"></canvas>
  </div>
</template>

<script>
export default {
  name: "FrequencyBarGraph",
  props: {
    canvasWidth: {
      type: Number,
      default: 750,
    },
    canvasHeight: {
      type: Number,
      default: 500,
    },
    fillStyle: {
      type: String,
      default: "rgba(100, 100, 140)",
    },
    strokeStyle: {
      type: String,
      default: "rgb(255, 255, 255)",
    },
  },
  data: function () {
    return {};
  },
  methods: {
    draw: function () {
      requestAnimationFrame(this.draw);

      this.analyser.getByteFrequencyData(this.dataArray);

      this.canvasCtx.fillStyle = this.fillStyle;
      this.canvasCtx.fillRect(0, 0, this.canvasWidth, this.canvasHeight);

      var barWidth = (this.canvasWidth / this.bufferLength) * 2.5;
      var barHeight;
      var x = 0;

      for (var i = 0; i < this.bufferLength; i++) {
        barHeight = this.dataArray[i] / 2;

        this.canvasCtx.fillStyle = "rgb(150," + (barHeight + 100) + "," + (barHeight + 100) + ")";
        this.canvasCtx.fillRect(
          x,
          this.canvasHeight - barHeight / 2,
          barWidth,
          barHeight
        );

        x += barWidth + 1;
      }
    },
  },
  mounted: function () {
    // Reference implementation from Mozilla's MDN
    // https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API/Visualizations_with_Web_Audio_API#creating_a_frequency_bar_graph

    navigator.mediaDevices
      .getUserMedia({ audio: true })
      .then((stream) => {
        this.audioCtx = new (window.AudioContext ||
          window.webkitAudioContext)();
        this.analyser = this.audioCtx.createAnalyser();

        this.source = this.audioCtx.createMediaStreamSource(stream);
        this.source.connect(this.analyser);

        this.analyser.fftSize = 256;

        this.bufferLength = this.analyser.frequencyBinCount;
        this.dataArray = new Uint8Array(this.bufferLength);

        // unique canvas identifier using component ID
        this.canvas = document.getElementById(this._uid);
        this.canvasCtx = this.canvas.getContext("2d");

        this.canvasCtx.clearRect(0, 0, this.canvasWidth, this.canvasHeight);

        this.draw();
      })
      .catch(function (err) {
        console.error(err);
      });
  },
};
</script>
