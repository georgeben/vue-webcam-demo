<template>
  <div>
    <div class="container">
      <div class="video-container">
        <video ref="video" class="video" id="video" width="853" height="480" autoplay></video>
        <div>
          <button @click="takePhoto" class="capture">
            Capture
          </button>
        </div>
      </div>
      <div class="preview">
        <canvas ref="canvas" class="canvas" id="canvas" width="853" height="480"></canvas>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "camera",
  async mounted() {
    try {
      let stream = await navigator.mediaDevices.getUserMedia({
      audio: false,
      video: {
        width: 800,
        height: 600
      },
    });
    const video = this.$refs.video;
    video.srcObject = stream;
    } catch (error) {
      alert("Could not open camera");
      // TODO Handle error which could probably because the user did not grant the permission to use the camera
      console.log("Why", error)
    }
  },
  methods: {
    captureBytes() {
      const video = this.$refs.video;
      const canvas = this.$refs.canvas;
      const ctx = canvas.getContext('2d');
      ctx.drawImage(video, 0, 0);
      return canvas.toDataURL('image/png')
    },
    takePhoto() {
      const bytes = this.captureBytes();
      // bytes is the base64 representation of the captured image
      // You can use it to do whatever you want e.g set the src of the agent's photo
      console.log("Hmmmm", bytes);

    }
  }
}
</script>

<style>

</style>