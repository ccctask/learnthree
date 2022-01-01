<template>
  <v-col>
    <v-col class="justify-center">
      <v-container>
        <v-row>
          <canvas class="window"></canvas>
          <div id="gui_container"></div>
        </v-row>

      </v-container>
      <v-divider></v-divider>
      <v-row>
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
import * as dat from "dat.gui";
import * as THREE from "three";
import {OrbitControls} from "three/examples/jsm/controls/OrbitControls";

export default {
  name: "texture",
  mounted() {
    let size ={
      width: window.innerWidth * 0.5 ,
      height: window.innerHeight * 0.5
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

    const AmbientLight=new THREE.AmbientLight(0xffffff,0.5)
    const pointLight=new THREE.PointLight("yellow",0.5)
    pointLight.castShadow=true
    const pointLightHelper=new THREE.PointLightHelper(pointLight)
    pointLight.position.y=1
    pointLight.position.z=0.5

    const fog =new THREE.Fog('#262837',1,6)

    const plan =new THREE.Mesh(
      new THREE.PlaneGeometry(15,15,10,10),
      new THREE.MeshStandardMaterial({})
    )
    plan.receiveShadow=true
    plan.position.z=-1
    plan.rotation.x=Math.PI * 1.5
    scene.add(AmbientLight,pointLight,plan)
    /*TODO CODE HERE*/



    scene.fog=fog
    // 开始渲染
    const renderer = new THREE.WebGLRenderer({
      canvas: canvas,
    })
    renderer.setSize(size.width,size.height)
    renderer.setPixelRatio(window.devicePixelRatio)
    renderer.shadowMap.enabled=true
    renderer.setClearColor('#262837')


    const clock=new THREE.Clock()
    const loop =()=> {
      //移动灯光
      const elapsed_time = clock.getElapsedTime()
      pointLight.position.x=Math.cos(elapsed_time)*2
      pointLight.position.z=Math.sin(elapsed_time)*2
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
