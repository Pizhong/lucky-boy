<!--
 * @file: 
-->
<template>
  <div>
    <canvas id="three"></canvas>
  </div>
</template>

<script>
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
export default {
  async mounted() {
    await this.initThree()
  },
  methods: {
    initThree() {
      const scene = new THREE.Scene()
      const canvas = document.querySelector('#three')
      // var geometry = new THREE.SphereGeometry(60, 40, 40)
      var geometry = new THREE.BoxGeometry(100, 100, 100); //创建一个立方体几何对象Geometry
      var textureLoader = new THREE.TextureLoader()
      var texture = textureLoader.load('src/assets/img/2.jpg', texture => {
         var material = new THREE.MeshLambertMaterial({
        // color: 0x0000ff,
        // opacity:0.7,
        // transparent:true
        specular:0x4488ee,
        shininess:12,
        map:texture
      }); 
      var mesh = new THREE.Mesh(geometry, material);
      scene.add(mesh);

      var point = new THREE.PointLight(0xffffff);
      point.position.set(400, 200, 300); //点光源位置
      scene.add(point); //点光源添加到场景中

      //环境光
      var ambient = new THREE.AmbientLight(0x444444);
      scene.add(ambient);

      var width = window.innerWidth; //窗口宽度
      var height = window.innerHeight; //窗口高度
      var k = width / height; //窗口宽高比
      var s = 400; //三维场景显示范围控制系数，系数越大，显示的范围越大

      // //创建相机对象
      var camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
      camera.position.set(200, 300, 200); //设置相机位置
      camera.lookAt(scene.position); //设置相机方向(指向的场景对象)

      var renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
      renderer.setSize(width, height);//设置渲染区域尺寸
      renderer.setClearColor(0xb9d3ff, 1); //设置背景颜色
      // // document.body.appendChild(renderer.domElement); //body元素中插入canvas对象
      // //执行渲染操作   指定场景、相机作为参数
      // renderer.render(scene, camera);
      // let T0 = new Date();//上次时间
      function render() {
        // let T1 = new Date();//本次时间
        // let t = T1-T0;//时间差
        // T0 = T1;//把本次时间赋值给上次时间
        // requestAnimationFrame(render);
        renderer.render(scene,camera);//执行渲染操作
        // mesh.rotateY(0.001*t);//旋转角速度0.001弧度每毫秒
      }
      render();
      const controls = new OrbitControls(camera, renderer.domElement)
      controls.addEventListener('change', render);//监听鼠标、键盘事件
      })



      // var scene = new THREE.Scene();
      // var camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);
      // const canvas = document.querySelector('#three')
      // var renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
      // renderer.setClearColor(0xEEEEEE);
      // renderer.setSize(window.innerWidth, window.innerHeight);
      // var sphereGeometry = new THREE.SphereGeometry(10, 20, 20)
      // var textureLoader = new THREE.TextureLoader()
      // const bg = textureLoader.load('src/assets/img/2.jpg')
      // var material = new THREE.PointsMaterial({
      //   color:Math.random()*0xffffff,
      //   size:0.5,
      //   // map:bg,
      //   blending: THREE.AdditiveBlending,
      //   depthTest: false,
      //   transparent: true,
      //   })
      // var points = new THREE.Points(sphereGeometry,material)
      // scene.add(points)
      // camera.position.x = -30;
      // camera.position.y = 40;
      // camera.position.z = 30;
      // camera.lookAt(scene.position);
      // const draw = function(){
      //   requestAnimationFrame(draw)//不断绘制
      //   renderer.render(scene, camera);//渲染
        
      //   points.rotation.y+=0.01//点云角度旋转
      // }
      // draw()
      // const controls = new OrbitControls(camera, renderer.domElement)
      // controls.addEventListener('change', draw);//监听鼠标、键盘事件
    },
  },
}
</script>

<style>
#three {
  width: 100%;
  height: 100%;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
}
</style>