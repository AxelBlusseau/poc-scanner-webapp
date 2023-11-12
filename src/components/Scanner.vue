<template>
  <div class="container" ref="container">
    <video ref="scanner" autoplay></video>
    <canvas ref="canvas" style="display: none"></canvas>
  </div>
</template>

<script>
import jsQR from "jsqr";

export default {
  name: "ScannerComp",
  components: {},
  data() {
    return {
      video: undefined,
    };
  },
  methods: {
    startCamera() {
      const videoElement = this.$refs.scanner;

      navigator.mediaDevices
        .getUserMedia({
          video: {
            width: 1280,
            height: 720,
            facingMode: { exact: "environment" },
          },
          audio: false,
        })
        .then((stream) => {
          videoElement.setAttribute("playsinline", "true");
          videoElement.srcObject = stream;
          videoElement.play();

          this.video = videoElement;
        })
        .catch((error) => {
          console.error("Error accessing camera:", error);
        });
    },

    scanQRCode() {
      const canvas = this.$refs.canvas;
      const canvasContext = canvas.getContext("2d", {
        willReadFrequently: true,
      });

      canvas.width = this.video.offsetWidth;
      canvas.height = this.video.offsetHeight;
      canvasContext.drawImage(this.video, 0, 0, canvas.width, canvas.height);

      const imageData = canvasContext.getImageData(
        0,
        0,
        canvas.width,
        canvas.height
      );
      const code = jsQR(imageData.data, imageData.width, imageData.height, {
        inversionAttempts: "dontInvert",
      });

      if (code) {
        return code.data;
      }
    },
  },
  mounted() {
    this.startCamera();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  border-radius: 10px;
  background: black;
  width: 100%;
  height: 100%;
}

video,
canvas {
  border-radius: 10px;
  object-fit: cover;
  width: 100%;
  height: 100%;
}
</style>
