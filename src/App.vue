<template>
  <div class="w-3/4 m-auto">
    <div>
      <h1 class="text-2xl text-green-800">Object detection with Tensorflow</h1>
      <div class="bg-gray-300 h-64 w-64 rounded-lg shadow-md bg-cover bg-center">
        <img class="w-64" ref="imgRef" src="https://images.unsplash.com/photo-1503792501406-2c40da09e1e2?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=752&q=80"
         alt="sciscors" crossorigin="anonymous">
      </div>
      <button @click="detect" class="focus:outline-none text-white text-sm py-2.5 px-5 rounded-md bg-gray-700 hover:bg-gray-900 hover:shadow-lg">Detect</button>
      <div v-if="result.length > 0">
              <p>{{ result[0].class }}</p>
          </div>
    </div>
  </div>
</template>

<script lang="ts">
import { ref } from "vue";
import { defineComponent } from 'vue';
require('@tensorflow/tfjs-backend-cpu')
require('@tensorflow/tfjs-backend-webgl')
const cocoSsd = require('@tensorflow-models/coco-ssd')
export default defineComponent({
  name: 'App',
  setup(){
    const imgRef = ref("");
     const result = ref([]);

    async function detect() {
      const img = imgRef.value;
      const model = await cocoSsd.load();
      const predictions = await model.detect(img);
      result.value = predictions;
      console.log(predictions, img);
    }  
  
    return {
      imgRef,
      detect,
      result
    }
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
