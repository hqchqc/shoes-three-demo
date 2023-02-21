<template>
  <div
    class="w-84 h-180 bg-[#e9e5e1] absolute top-1/2 left-1/2 translate-[-50%] rounded-8 overflow-hidden"
  >
    <header class="pl-8 pt-3">
      <h6 class="m-0">{{ getCurrentTime() }}</h6>
    </header>

    <hgroup class="pl-10">
      <h1 class="mb-0">AIR MAX 90</h1>
      <h6 class="mt-2 mb-0 font-medium">25 items</h6>
    </hgroup>

    <section class="pl-10 pr-10">
      <div class="bg-white">
        <canvas id="three" class="w-full mt-10"></canvas>

        <span>21321</span>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import {
  Color,
  DirectionalLight,
  HemisphereLight,
  Mesh,
  MeshBasicMaterial,
  MeshPhongMaterial,
  PerspectiveCamera,
  PlaneGeometry,
  Scene,
  TextureLoader,
  Vector2,
  WebGLRenderer,
} from "three";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { onMounted } from "vue";

let currentTime = "9:41";

const getCurrentTime = () => {
  const date = new Date();
  return `${date.getHours()}:${date.getMinutes()}`;
};

const initThree = () => {
  console.log("1", 1);
  const scene = new Scene();
  scene.background = new Color("#dfd8d5");
  const canvas = document.querySelector("#three") as HTMLCanvasElement;
  const renderer = new WebGLRenderer({ canvas, antialias: true });
  const camera = new PerspectiveCamera(
    105,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.y = 10;
  camera.position.z = 5;

  const loader = new GLTFLoader().setPath("/shoes/");
  // loader.load("scene.gltf", (gltf) => {
  //   gltf.scene.position.z = 0.01;

  //   gltf.scene.traverse((child) => {
  //     const explosionTexture = new TextureLoader().load(
  //       "/shoes/textures/material_0_diffuse.jpeg"
  //     );

  //     explosionTexture.flipY = false;

  //     const material = new MeshBasicMaterial({
  //       map: explosionTexture,
  //     });

  //     (child as Mesh).material = material;
  //   });

  //   gltf.scene.rotateX(-Math.PI / 2);
  //   gltf.scene.rotateY(Math.PI / 2);
  //   gltf.scene.translateY(-3);
  //   gltf.scene.translateZ(-1);
  //   // gltf.scene.rotateZ(Math.PI * 2);
  //   // gltf.scene.rotateX(Math.PI / 4);

  //   scene.add(gltf.scene);
  // });

  // loader.load("scene.gltf", (gltf) => {
  //   gltf.scene.position.z = 0.01;

  //   gltf.scene.traverse((child) => {
  //     const explosionTexture = new TextureLoader().load(
  //       "/shoes/textures/material_0_diffuse.jpeg"
  //     );

  //     explosionTexture.flipY = false;

  //     const material = new MeshBasicMaterial({
  //       map: explosionTexture,
  //     });

  //     (child as Mesh).material = material;
  //   });

  //   gltf.scene.translateX(1);
  //   gltf.scene.translateZ(1);
  //   gltf.scene.rotateX(Math.PI / 2);
  //   gltf.scene.rotateY(-Math.PI / 2);

  //   scene.add(gltf.scene);
  // });

  // const boxLoader = new GLTFLoader().setPath("/box/");
  // boxLoader.load("scene.gltf", (gltf) => {
  //   gltf.scene.traverse((child) => {
  //     const explosionTexture = new TextureLoader().load(
  //       "/box/textures/BOTTOM_baseColor.png"
  //     );

  //     if (child.name === "Plane_Plane_002_Material_001_TOP_0") {
  //       const lidTexture = new TextureLoader().load(
  //         "/box/textures/material_baseColor.png"
  //       );
  //       lidTexture.flipY = false;
  //       const material = new MeshBasicMaterial({
  //         map: lidTexture,
  //       });
  //       (child as Mesh).material = material;

  //       child.rotateX(-(Math.PI * 3) / 4);

  //       return;
  //     }

  //     console.log("child", child);
  //     explosionTexture.flipY = false;

  //     const material = new MeshBasicMaterial({
  //       map: explosionTexture,
  //     });

  //     (child as Mesh).material = material;
  //   });

  //   gltf.scene.translateY(-2);
  //   gltf.scene.translateZ(2);
  //   gltf.scene.scale.set(22, 22, 22);
  //   scene.add(gltf.scene);
  // });

  const controls = new OrbitControls(camera, renderer.domElement);
  controls.enableDamping = true;

  function animate() {
    renderer.render(scene, camera);
    requestAnimationFrame(animate);

    if (resizeRendererToDisplaySize(renderer)) {
      const canvas = renderer.domElement;
      camera.aspect = canvas.clientWidth / canvas.clientHeight;
      camera.updateProjectionMatrix();
    }
  }

  animate();

  function resizeRendererToDisplaySize(renderer: WebGLRenderer) {
    const canvas = renderer.domElement;
    var width = window.innerWidth;
    var height = window.innerHeight;
    var canvasPixelWidth = canvas.width / window.devicePixelRatio;
    var canvasPixelHeight = canvas.height / window.devicePixelRatio;

    const needResize =
      canvasPixelWidth !== width || canvasPixelHeight !== height;
    if (needResize) {
      renderer.setSize(width, height, false);
    }
    return needResize;
  }
};

onMounted(() => {
  initThree();
});
</script>
