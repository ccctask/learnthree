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


    // AmbientLight 节能  1
    const AmbientLight=new THREE.AmbientLight(0xffffff,0.5)
    /*4*/
    const pointlight=new THREE.PointLight("red",0.5,10,3)
    pointlight.visible=false
    pointlight.position.y=2
    pointlight.position.x=-4
    /*3*/
    const directionalLight=new THREE.DirectionalLight("yellow",0.5)
    directionalLight.visible=false
    directionalLight.position.y=2
    directionalLight.position.x=2
    /*hemisphereLight 节能2 */
    const hemisphereLight=new THREE.HemisphereLight("blue",0.5)
    hemisphereLight.visible=false
    hemisphereLight.position.y=2
    const rectAreaLight =new THREE.RectAreaLight("pink",0.5,1,1)
    rectAreaLight.visible=false
    rectAreaLight.position.y=2
    const spotLight=new THREE.SpotLight("green",0.5,1,2,3,3)
    spotLight.visible=false
    spotLight.position.y=4
    spotLight.position.z=-2
    spotLight.target.position.x=0.75
    window.requestAnimationFrame(()=>{
      spotLightHelper.update()
    })

    const pointlightHelper = new THREE.PointLightHelper(pointlight)
    const directionalLightHelper=new THREE.DirectionalLightHelper(directionalLight)
    const hemisphereLightHelper=new THREE.HemisphereLightHelper(hemisphereLight)
    const spotLightHelper=new THREE.SpotLightHelper(spotLight)
    scene.add(AmbientLight,pointlight,directionalLight,hemisphereLight,spotLight,spotLightHelper,rectAreaLight,pointlightHelper,directionalLightHelper,hemisphereLightHelper)

    const plan =new THREE.Mesh(
      new THREE.PlaneGeometry(20,10,10),
      new THREE.MeshStandardMaterial({roughness:0.5})
    )
    plan.rotation.x=Math.PI * 1.5

    const cube_standard =new THREE.Mesh(
      new THREE.BoxGeometry(1,1,1),
      new THREE.MeshStandardMaterial({roughness:0.5})
    )
    cube_standard.position.y=1
    cube_standard.visible=true

    const sphere_standard=new THREE.Mesh(
      new THREE.SphereGeometry(0.5,30,16),
      new THREE.MeshStandardMaterial({roughness:0.4})
    )
    sphere_standard.position.x=-2
    sphere_standard.position.y=1

    scene.add(plan,cube_standard,sphere_standard)

    /*GUI*/
    const ambientLight_ui_group=gui.addFolder("ambientiLight")
    ambientLight_ui_group.add(AmbientLight,'visible').name("开关")

    const rectareaLight_ui_group=gui.addFolder("rectAreaLight")
    rectareaLight_ui_group.add(rectAreaLight,'visible').name("开关")
    rectareaLight_ui_group.add(rectAreaLight,'intensity',-5,5,0.1).name("强度")
    rectareaLight_ui_group.add(rectAreaLight,'width',-2,2,0.1).name("宽")
    rectareaLight_ui_group.add(rectAreaLight,'height',-2,2,0.1).name("高")


    const pointlight_ui_group=gui.addFolder("pointLight")
    pointlight_ui_group.add(pointlight,'visible').name("开关")
    pointlight_ui_group.add(pointlightHelper,'visible').name("灯光辅助线")
    pointlight_ui_group.add(pointlight,'intensity',-5,5,0.1).name("强度")
    pointlight_ui_group.add(pointlight.position,'y',-2,2,0.1).name("高度")
    pointlight_ui_group.add(pointlight,'distance',-5,5,0.1).name("范围")
    pointlight_ui_group.add(pointlight,'decay',-5,5,0.1).name("衰减")


    const spotlight_ui_group=gui.addFolder("sportLight")
    spotlight_ui_group.add(spotLight,'visible').name("开关")
    spotlight_ui_group.add(spotLightHelper,'visible').name("灯光辅助线")
    spotlight_ui_group.add(spotLight,'intensity',-5,5,0.1).name("强度")
    spotlight_ui_group.add(spotLight.position,'y',-10,10,0.1).name("高度")
    spotlight_ui_group.add(spotLight,'distance',-5,5,0.1).name("范围")
    spotlight_ui_group.add(spotLight,'angle',0,Math.PI/2,0.1).name("散射角度")
    spotlight_ui_group.add(spotLight,'penumbra',0,1,0.1).name("半影衰减百分比")
    spotlight_ui_group.add(spotLight,'decay',0,10,0.1).name("照多远")


    const directionalLight_ui_group=gui.addFolder("directionalLight")
    directionalLight_ui_group.add(directionalLight,'visible').name("开关")
    directionalLight_ui_group.add(directionalLightHelper,'visible').name("灯光辅助线")
    directionalLight_ui_group.add(directionalLight,'intensity',-5,5,0.1).name("强度")
    directionalLight_ui_group.add(directionalLight.position,'y',-1,1,0.1).name("高度")


    const hemisphereLight_ui_group=gui.addFolder("hemisphereLight")
    hemisphereLight_ui_group.add(hemisphereLight,'visible').name("开关")
    hemisphereLight_ui_group.add(hemisphereLightHelper,'visible').name("灯光辅助线")
    hemisphereLight_ui_group.add(hemisphereLight.position,'y',-1,1,0.1).name("高度")
    hemisphereLight_ui_group.add(hemisphereLight,'intensity',-5,5,0.1).name("强度")



    const sphere_ui_group=gui.addFolder("MeshBasicMaterial")
    sphere_ui_group.add(cube_standard,'visible').name("是否显示")
    sphere_ui_group.add(cube_standard.material,'wireframe').name("wireframe 网格")
    sphere_ui_group.add(cube_standard.material,'transparent').name("transparent 透明开关")
    sphere_ui_group.add(cube_standard.material,'opacity',-1,1,0.01).name("transparent 不透明度")


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
