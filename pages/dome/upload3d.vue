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
          <v-file-input
            v-model="files"
            color="deep-purple accent-4"
            counter
            label="File input"
            multiple
            placeholder="Select your files"
            prepend-icon="mdi-paperclip"
            outlined
            :show-size="1000"
          >
            <template v-slot:selection="{ index, text }">
              <v-chip
                v-if="index < 2"
                color="deep-purple accent-4"
                dark
                label
                small
              >
                {{ text }}
              </v-chip>

              <span
                v-else-if="index === 2"
                class="text-overline grey--text text--darken-3 mx-2"
              >
        +{{ files.length - 2 }} File(s)
      </span>
            </template>
          </v-file-input>
        </v-container>
        <v-card-actions>
          <v-btn @click="getModules">模型上传</v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-col>

</template>

<script>
import * as dat from "dat.gui";
import * as THREE from "three";
import {OrbitControls} from "three/examples/jsm/controls/OrbitControls";
import { STLLoader } from 'three/examples/jsm/loaders/STLLoader'

export default {
  name: "upload3d",
  data: () => ({
    files: [],
    myscene: THREE.Scene
  }),
  mounted() {
    let size ={
      width: window.innerWidth * 0.5 ,
      height: window.innerHeight * 0.5
    }
    const canvas = document.querySelector('.window')
    const gui = new dat.GUI({ autoPlace: false });
    gui.domElement.id = 'gui';
    gui_container.appendChild(gui.domElement);

    // const scene = new THREE.Scene();
    this.myscene = new THREE.Scene()

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
    // scene.add(AmbientLight,pointLight,plan)
    this.myscene.add(AmbientLight,pointLight,plan)
    /*TODO CODE HERE*/


    const loader = new STLLoader()
    const material = new THREE.MeshLambertMaterial({
      color:"red"
    })
    const _this=this
    loader.load(
      '/modules/CatTheInnerWay.stl',
      function (geometry) {
        const mesh = new THREE.Mesh(geometry, material)
        _this.myscene.add(mesh)
      },
      (xhr) => {
        console.log((xhr.loaded / xhr.total) * 100 + '% loaded')
      },
      (error) => {
        console.log(error)
      }
    )



    // scene.fog=fog
    // this.myscene.fog=fog
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
      // renderer.render(scene,camera)
      renderer.render(this.myscene,camera)
      window.requestAnimationFrame(loop)
    }
    loop()


  },
  methods:{
    getModules(){
      console.log(this.files)
    }
  }
}
</script>

<style scoped>

</style>
