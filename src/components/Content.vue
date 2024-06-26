<template>
  <div id="container"></div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted } from "vue";
import * as THREE from "three";
import {
  CylinderGeometry,
  MeshBasicMaterial,
  Mesh,
  PerspectiveCamera,
  Scene,
  WebGLRenderer,
  AmbientLight,
} from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";

let camera: PerspectiveCamera, scene: Scene, renderer: WebGLRenderer;
let controls: OrbitControls;
let cylinder: Mesh;

onMounted(() => {
  init();
  animate();
});

onUnmounted(() => {
  renderer.dispose();
});

function init() {
  const container = document.getElementById("container");

  // Scene
  scene = new THREE.Scene();

  // Camera
  camera = new THREE.PerspectiveCamera(
    45,
    window.innerWidth / window.innerHeight,
    1,
    1000
  );
  camera.position.set(0, 150, 400);
  scene.add(camera);

  // Renderer
  renderer = new THREE.WebGLRenderer();
  renderer.setSize(window.innerWidth, window.innerHeight);
  container!.appendChild(renderer.domElement);

  // Human model
  const geometry = new CylinderGeometry(50, 50, 200, 32);
  const material = new MeshBasicMaterial({ color: 0x808080 });
  cylinder = new Mesh(geometry, material);
  scene.add(cylinder);

  // Light
  const ambientLight = new AmbientLight(0x404040); // soft white light
  scene.add(ambientLight);

  // Controls
  controls = new OrbitControls(camera, renderer.domElement);
  controls.addEventListener("change", render); // use if there is no animation loop
  controls.minDistance = 200;
  controls.maxDistance = 500;
  controls.enablePan = false;
}

function animate() {
  requestAnimationFrame(animate);
  controls.update(); // only required if controls.enableDamping = true, or if controls.autoRotate = true
  render();
}

function render() {
  renderer.render(scene, camera);
}
</script>

<style lang="scss" scoped>
#container {
  width: 100%;
  height: 100vh;
  display: block;
}
</style>
