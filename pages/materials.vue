<template>
  <v-col>
    <v-col class="justify-center">
      <canvas class="window"></canvas>
      <div id="gui_container"></div>
    </v-col>
    <v-col>
      <v-card>
        <v-container>
          <v-card-title>hello world </v-card-title>
        </v-container>
      </v-card>
    </v-col>
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
      width: window.innerWidth * 0.6 ,
      height: window.innerHeight * 0.6
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
    sphere.visible=false
    scene.add(camera,sphere)

    const sphere_ui_group=gui.addFolder("sphere")
    sphere_ui_group.add(sphere,'visible').name("是否显示")
    sphere_ui_group.add(sphere.material,'wireframe').name("wireframe 网格")




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
