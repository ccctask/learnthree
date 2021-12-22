<template>
  <v-col>
    <canvas class="window"></canvas>
  </v-col>
</template>

<script>
import * as THREE from 'three'
import {  OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

export default {
  name: "materials",
  data () {
    return{
      canvas: new Document(),
      scene: new THREE.Scene(),
      windowSize: {
        width: window.innerWidth *0.5,
        height: window.innerHeight *0.5,
      },
      // camera: new THREE.PerspectiveCamera(70,this.windowSize.width / this.windowSize.height)
    }
  },
  mounted (){
    const funcc=()=>{
      console.log(this.windowSize)
    }
    funcc()
  }
}
</script>

<style scoped>

</style>
