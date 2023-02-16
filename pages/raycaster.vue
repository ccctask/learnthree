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
            射线检测
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
  name: "raycaster",
  mounted() {
    let size ={
      width: window.innerWidth * 0.5 ,
      height: window.innerHeight * 0.5
    }
    const canvas = document.querySelector('.window')
    const gui = new dat.GUI({ autoPlace: false });
    //GUI
    gui.domElement.id = 'gui';
    gui_container.appendChild(gui.domElement);

    //场景  摄像头 和控制器
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(70,size.width / size.height)
    const control= new OrbitControls(camera,canvas)
    control.enableDamping=true
    camera.position.set(1,1,3);

    //灯光
    const AmbientLight=new THREE.AmbientLight(0xffffff,0.5)
    const pointLight=new THREE.PointLight("yellow",0.5)
    pointLight.castShadow=true
    const pointLightHelper=new THREE.PointLightHelper(pointLight)
    pointLight.position.y=1
    pointLight.position.z=0.5

    const fog =new THREE.Fog('#262837',1,6)

    scene.add(AmbientLight,pointLight)
    /*TODO CODE HERE*/


    const object1 = new THREE.Mesh(
      new THREE.SphereBufferGeometry(0.5,16,16),
      new THREE.MeshBasicMaterial({color:"yellow"})
    )
    object1.position.x=-2

    const object2 = new THREE.Mesh(
      new THREE.SphereBufferGeometry(0.5,16,16),
      new THREE.MeshBasicMaterial({color:"yellow"})
    )


    const object3 = new THREE.Mesh(
      new THREE.SphereBufferGeometry(0.5,16,16),
      new THREE.MeshBasicMaterial({color:"yellow"})
    )
    object3.position.x=2

    scene.add(object1,object2,object3)


    //设置射线
    const raycaster = new THREE.Raycaster();
    const pointer = new THREE.Vector2();

    function onPointerMove( event ) {

      // calculate pointer position in normalized device coordinates
      // (-1 to +1) for both components

      pointer.x = ( event.clientX / size.width ) * 2 - 1;
      pointer.y = - ( event.clientY / size.height ) * 2 + 1;
      console.log(pointer)
    }
    function render() {

      // update the picking ray with the camera and pointer position
      raycaster.setFromCamera( pointer, camera );

      // calculate objects intersecting the picking ray
      const intersects = raycaster.intersectObjects( scene.children );

      for ( let i = 0; i < intersects.length; i ++ ) {

        intersects[ i ].object.material.color.set( 0xff0000 );

      }

      renderer.render( scene, camera );

    }

    window.addEventListener( 'pointermove', onPointerMove );

    window.requestAnimationFrame(render);


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
