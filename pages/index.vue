<template>
  <section>
    <h1 class="header">{{ title }}</h1>
    <div class="picture-input">
      <input
        type="file"
        @change="onFileSelected"
        style="display: none"
        ref="fileInput"
        accept="image/*"
        id="fileInput"
      />
      <button @click="$refs.fileInput.click()" class="fileInputButton">
        사진 업로드
      </button>
    </div>
    <img v-if="url" :src="url" height="300px" width="300px" />
    <div class="result" v-if="url">
      {{ result() }}
    </div>
    <button
      @click="samplePostFunction"
      style="width: 400px; height: 400px;"
    ></button>
  </section>
</template>

<script lang="ts">
import { Component, Vue } from 'nuxt-property-decorator';
import axios from 'axios';
import { State } from 'vuex-class';

@Component({
  components: {},
})
export default class IndexPage extends Vue {
  title: string = 'Party Prediction';
  url: string = '';

  selectedFile: any = null;

  async samplePostFunction() {
    console.log('this.selectedFile', this.selectedFile);
    let formData = new FormData();
    formData.append('file', this.selectedFile);
    const data = await axios.post('http://localhost:5000/sample', formData, {
      headers: {
        'Content-Type': 'multipart/form-data',
      },
    });
    console.log('after axios post data', data);
  }

  onFileSelected(event: any) {
    console.log('this is', this);
    this.selectedFile = event.target.files[0];
    this.url = URL.createObjectURL(this.selectedFile);
    const that = this.url;
    // setTimeout(function() {
    //   URL.revokeObjectURL(that);
    // }, 1000);
  }

  testThis = () => {
    console.log('testThis', this);
    this.innerFunc();
  };

  innerFunc() {
    console.log('innerFunc', this);
  }

  result() {
    return 'hello!';
  }
}
</script>

<style scoped>
section {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.header {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
.fileInputButton {
  width: 200px;
  height: 100px;
  border-radius: 30px;
  font-size: 30px;
}
img {
  margin-top: 10px;
}
.result {
  font-size: 40px;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}
</style>
