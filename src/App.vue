<template>
  <div>
    <input type="file" @change="onChange" accept="image/*">
    <button v-if="image" @click="upload">Upload</button>
    <br>
    <img src="https://zippy.gfycat.com/SkinnySeveralAsianlion.gif" width="16" height="16" v-show="loading">
    <div v-show="url">
      <a :href="url" target="_blank">{{ url }}</a>
    </div>
    <div v-show="image">
      <img :src="image">
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'app',
  data() {
    return {
      image: null,
      url: null,
      loading: false,
    };
  },
  methods: {
    onChange() {
      this.image = this.url = null;
      this.previewImage(event);
    },
    previewImage(e) {
      const input = e.target.files[0];
      if (input) {
        const reader = new FileReader();
        reader.onload = () => {
          this.image = event.target.result;
        };
        reader.readAsDataURL(input);
      }
    },
    upload() {
      this.loading = true;
      axios
        .post(
          'https://api.imgur.com/3/image',
          { image: this.image.split(',')[1] },
          {
            headers: { Authorization: 'Client-ID 14fd0da0bdb0160' },
          },
        )
        .then(({ data }) => {
          this.url = data.data.link;
          this.loading = false;
        })
        .catch(() => {
          this.loading = false;
        });
    },
  },
};
</script>
