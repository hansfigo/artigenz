<script>
  import * as THREE from "three";
  import { OrbitControls } from "three/addons/controls/OrbitControls.js";
  import { GLTFLoader } from "three/addons/loaders/GLTFLoader.js";

  import Shiroko from "../assets/shiroko.glb";

  let container;

  const scene = new THREE.Scene();

  const camera = new THREE.PerspectiveCamera(75, 300 / 300, 0.1, 1000);

  const geometry = new THREE.BoxGeometry(1, 1, 1);
  const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
  const cube = new THREE.Mesh(geometry, material);

  const loader = new GLTFLoader();
  loader.load(Shiroko, (gltf) => {
    console.log(gltf, "gltf");
    scene.add(gltf.scene);
    const model = gltf.scene;

    model.scale.set(2, 2, 2);

    model.position.set(0, -0.75, 0);
  });

  camera.position.z = 1.5;

  camera.lookAt(0, 0, 0);

  const renderer = new THREE.WebGLRenderer({ alpha: true });

  const light = new THREE.AmbientLight(0x7678ab, 14);
  scene.add(light);

  $: if (container) {
    console.log(container.clientWidth, container.clientHeight);
    renderer.setSize(container.clientWidth, container.clientHeight);
    container.appendChild(renderer.domElement);
  }

  const controls = new OrbitControls(camera, renderer.domElement);

  controls.enableZoom = false;
  controls.enablePan = false;

  controls.touches.ONE = THREE.TOUCH.PAN;
  controls.touches.TWO = THREE.TOUCH.DOLLY_ROTATE;

  function animate() {
    renderer.render(scene, camera);

    if (scene.children.length > 0) {
      if (scene.children[1]) {
        const model = scene.children[1];
        model.rotation.y += 0.002;
      }
    }
  }
  renderer.setAnimationLoop(animate);
</script>

<div
  class="min-h-[48rem] min-w-full"
  bind:this={container}
  style="width: 100%; height: 100%;"
></div>
