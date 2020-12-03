<template>
  <div class="about">
    <input type="text" v-model="videoTitle" />
    <input type="file" ref="file" @change="videFileChange" />
    <button @click="getCredentials()">upload</button>
    <button @click="getStatusOfAVideo()">getStatus</button>
    <div>
      <VuejsonPretty
      :path="'res'"
      :data="status"
      :deep="Math.Infinity"
      :showLine="true"
      >
    </VuejsonPretty>
    </div>
  </div>
</template>

<script>
import VuejsonPretty from 'vue-json-pretty';
import axios from "axios";
export default {
  components: {
    VuejsonPretty
  },
  data() {
    return {
      videFile: null,
      videoTitle: "",
      videoId: null,
      status: null,
    };
  },
  methods: {
    getCredentials() {
      axios
        .get(
          "http://localhost:3001/video-upload/get-credentials/" +
            this.videoTitle
        )
        .then((res) => {
          console.log(res.data.videoId);
          this.uploadVideo(res.data.videoId);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    uploadVideo(id) {
      this.videoId = id;
      let formData = new FormData();
      formData.append("videofile", this.videFile);
      formData.append("videoId", this.videoId);
      axios
        .post(
          "http://localhost:3001/video-upload/upload-client-video",
          formData,
          {
            videoId: id
          },
          {
            headers: {
              "Content-Type": "multipart/form-data",
            },
          }
        )
        .then((res) => {
          console.log(res.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    getStatusOfAVideo() {
      //https://dev.vdocipher.com/api/videos/{{videoID}}
      axios.get("http://localhost:3001/video-upload/uploaded-video-satatus/" + this.videoId).then(res => {
        this.status = JSON.parse(res.data.body);
        let b = {
          id: this.status.id,
          title: this.status.title,
          description: this.description,
          upload_time: new Date(this.status.upload_time),
          length: this.status.length,
          status: this.status.status,
          public: this.status.public,
          tSize: Math.floor((this.status.totalSizeBytes / 1024)/ 1024)+ ' MB',
          poster: this.status.poster
        }
        this.status = b;
        b = {}
        console.log(this.status)
      }).catch(err => {
        console.log(err);
      });
    },
    videFileChange() {
      this.videFile = this.$refs.file.files[0];
      console.log(this.videFile);
    },
  },
};
</script>

<style>
</style>script>
