<template>
  <v-col>
    <canvas class="window"></canvas>
    <div id="gui_container"></div>
  </v-col>
</template>

<script>
import * as THREE from 'three'
import {  OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'
import * as dat from 'dat.gui'

export default {
  name: "materials",
  data () {
    return{}
  },
  mounted () {
    let size ={
      width: window.innerWidth *0.5,
      height: window.innerHeight *0.5
    }
    const canvas = document.querySelector('.window')
    var gui = new dat.GUI({ autoPlace: false });
    gui.domElement.id = 'gui';
    gui_container.appendChild(gui.domElement);
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(70,size.width / size.height)
    const control= new OrbitControls(camera,canvas)
    control.enableDamping=true
    camera.position.set(1,1,3);


    const sphere =new THREE.Mesh(
      new THREE.SphereGeometry(0.5, 32, 16),
      new THREE.MeshBasicMaterial({color:"pink",wireframe:true})
    )
    scene.add(camera,sphere)

    const sphere_group=gui.addFolder("sphere")
    sphere_group.add(sphere.material,'wireframe').name("wireframe 网格")



    // 开始渲染
    const renderer = new THREE.WebGLRenderer({
      canvas: canvas,
    })
    renderer.setSize(size.width,size.height)
    renderer.setPixelRatio(window.devicePixelRatio)




    const loop =()=> {
      control.update()
      renderer.render(scene,camera)
      window.requestAnimationFrame(loop)
    }
    loop()

  }
}
</script>

<style scoped>

</style>
