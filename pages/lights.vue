<template>
  <v-col>
    <v-col class="justify-center">
      <v-container>
        <canvas class="window"></canvas>
      </v-container>
      <v-divider></v-divider>

      <v-row>
        <v-container class="ma-2">
          <div id="gui_container"></div>

        </v-container>
        <v-container>
          <v-card-title>
            hell
          </v-card-title>
        </v-container>
      </v-row>

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
  name: "lights",
  data () {
    return{}
  },
  mounted () {
    let size ={
      width: window.innerWidth * 0.6 ,
      height: window.innerHeight * 0.6
    }
    const canvas = document.querySelector('.window')
    const gui = new dat.GUI({ autoPlace: false });
    gui.domElement.id = 'gui';
    gui_container.appendChild(gui.domElement);
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(70,size.width / size.height)
    const control= new OrbitControls(camera,canvas)
    control.enableDamping=true
    camera.position.set(1,1,3);


    // 灯光
    const AmbientLight=new THREE.AmbientLight(0xffffff,0.5)
    const pointlight=new THREE.PointLight(0xffffff,0.5)

    const pointlightHelper = new THREE.PointLightHelper(pointlight)
    // pointlight.position.x=-5
    pointlight.position.y=2
    // pointlight.position.z=1
    scene.add(AmbientLight,pointlight,pointlightHelper)

    const cube_BasicMaterial =new THREE.Mesh(
      new THREE.BoxGeometry(1,1,1),
      new THREE.MeshBasicMaterial({color:"pink",wireframe:true})
    )
    cube_BasicMaterial.visible=false
    scene.add(camera,cube_BasicMaterial)

    /*GUI*/
    const sphere_ui_group=gui.addFolder("MeshBasicMaterial")
    sphere_ui_group.add(cube_BasicMaterial,'visible').name("是否显示")
    sphere_ui_group.add(cube_BasicMaterial.material,'wireframe').name("wireframe 网格")
    sphere_ui_group.add(cube_BasicMaterial.material,'transparent').name("transparent 透明开关")

    sphere_ui_group.add(cube_BasicMaterial.material,'opacity',-1,1,0.01).name("transparent 不透明度")


    //new THREE.SphereGeometry(0.5, 32, 16),


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
