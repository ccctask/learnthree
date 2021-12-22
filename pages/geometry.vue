<template>
  <v-card>
    <v-container >
      <canvas class="window"></canvas>
    </v-container>
  </v-card>
</template>

<script>
import * as THREE from 'three'
import {  OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js'

export default {
  name: "geometry",
  mounted() {
    let size ={
      // width: window.innerWidth,
      // height: window.innerHeight
      width: window.innerWidth *0.5,
      height: window.innerHeight *0.5
    }
    const canvas = document.querySelector('.window')
    // 添加场景
    const scene = new THREE.Scene();
    // 添加相机
    const camera = new THREE.PerspectiveCamera(70,size.width / size.height)
    const control= new OrbitControls(camera,canvas)
    control.enableDamping=true
    scene.add(camera)
    //不设定会导致 控制器不起效。可能是因为 无限远
    camera.position.set(1,1,3);

// 创建正方体
    const box_geometry = new THREE.BoxGeometry(1,1,1);
// 设置材质
    const box_material = new THREE.MeshBasicMaterial({color:"yellow"})
// 贴材质到正方体 =网格
    const box_mesh = new THREE.Mesh(box_geometry, box_material);
    scene.add(box_mesh);

    // 开始渲染
    const renderer = new THREE.WebGLRenderer({
      canvas: canvas,
    })
    // 渲染的大小
    renderer.setSize(size.width,size.height)
    // 像素毛刺
    renderer.setPixelRatio(window.devicePixelRatio)

    let clock =new THREE.Clock()
    const loop =()=> {
      let elapsedTime=clock.getElapsedTime()
      box_mesh.rotation.y = elapsedTime
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
