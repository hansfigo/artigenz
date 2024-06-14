<script>
  import { T } from "@threlte/core";
  import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
  import { useLoader } from "@threlte/core";

  const gltf = useLoader(GLTFLoader).load("/assets/shiroko.glb");
</script>

{#if $gltf}
  <T is={$gltf.scene} />
{/if}
