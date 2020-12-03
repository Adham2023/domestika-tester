<template>
  <div class="home">
    <p>List of videos</p>
    <ul>
      <li v-for="(videoId, index) in videoList" :key="index">
        <button @click="play(videoId)">{{ videoId }}</button>
      </li>
    </ul>
    <div class="video-box">
      <iframe
      :key="otp"
      :src="
        'https://player.vdocipher.com/playerAssets/1.x/vdo/embed/index.html#otp=' +
        otp +
        '&playbackInfo=' +
        playbackInfo
      "
      style="border: 0; height: 100%; width: 100%; max-width: 100%"
      allowFullScreen="true"
      allow="encrypted-media"
    ></iframe>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import axios from "axios";
export default {
  name: "Home",
  components: {
  },
  mounted() {
    let recaptchaScript = document.createElement("script");
    recaptchaScript.setAttribute(
      "src",
      "https://dev.vdocipher.com/playerAssets/1.6.10/vdo.js"
    );
    document.head.appendChild(recaptchaScript);
    this.getAllVideInfo();
  },
  data() {
    return {
      video: null,
      videoList: [],
      otp: null,
      playbackInfo: null,
    };
  },
  methods: {
    getAllVideInfo() {
      axios
        .get("http://localhost:3001/video-upload/get-all-stored-video")
        .then((res) => {
          res.data.forEach((video) => {
            this.videoList.push(video.videoId);
          });
        })
        .catch((err) => {
          console.log(err);
        });
    },
    getOPTPlaybackInfo(id) {
      return new Promise((resolve, reject) => {
        axios
          .post("http://localhost:3001/video-upload/get-otp/" + id)
          .then((res) => {
            this.otp = res.data.otp;
            this.playbackInfo = res.data.playbackInfo;
            resolve({ otp: res.data.otp, playbackInfo: res.data.playbackInfo });
          })
          .catch((err) => {
            reject(err);
            console.log(err);
          });
      });
    },
    resume() {
      this.video.play();
    },
    pause() {
      this.video.pause();
    },
    play(id) {
      this.getOPTPlaybackInfo(id)
        .then((res) => {
          this.video = this.createVideoInstance(id, res.otp);
          this.video.addEventListener("load", () => {
            this.video.play();
          });
        })
        .catch((err) => console.log(err));
    },
    
  },
};
</script>

<style scoped>
  .video-box {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    
  }
</style>