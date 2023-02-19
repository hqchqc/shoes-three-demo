<template>
  <div
    class="w-84 h-180 bg-white absolute top-1/2 left-1/2 translate-[-50%] rounded-8 overflow-hidden"
  >
    <canvas id="three" class="w-full mt-10"></canvas>
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
import { onMounted, render } from "vue";

const initThree = () => {
  console.log("1", 1);
  const scene = new Scene();
  scene.background = new Color("#eee");
  const canvas = document.querySelector("#three") as HTMLCanvasElement;
  const renderer = new WebGLRenderer({ canvas, antialias: true });
  const camera = new PerspectiveCamera(
    50,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.z = 10;

  const loader = new GLTFLoader().setPath("/shoes/");
  loader.load("scene.gltf", (gltf) => {
    gltf.scene.position.z = 0.8;

    gltf.scene.traverse((child) => {
      const explosionTexture = new TextureLoader().load(
        "/shoes/textures/material_0_diffuse.jpeg"
      );

      explosionTexture.flipY = false;

      const material = new MeshBasicMaterial({
        map: explosionTexture,
      });

      (child as Mesh).material = material;
    });

    scene.add(gltf.scene);
  });

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
