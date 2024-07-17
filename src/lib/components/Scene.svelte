<script lang="ts">
  import { T, useThrelte } from "@threlte/core";
  import { OrbitControls, Sky, useGltf } from "@threlte/extras";
  import { FogExp2 } from "three";
  import Trees from "./Trees.svelte";

  const count = 1000000;
  const terrainSize = 125000;

  const gltf = useGltf("/tree.glb");

  const { scene } = useThrelte();

  scene.fog = new FogExp2("white", 0.0004);
</script>

<T.PerspectiveCamera makeDefault position={[0, 100, 0]} fov={70} far={3000}>
  <OrbitControls target.y={1.5} target.z={3} />
</T.PerspectiveCamera>

<T.DirectionalLight intensity={0.8} position.x={5} position.y={10} />

<Sky scale={terrainSize} />

<T.Mesh receiveShadow rotation.x={Math.PI / -2}>
  <T.PlaneGeometry args={[terrainSize, terrainSize, 10, 10]} />
  <T.MeshLambertMaterial color={0x004622} />
</T.Mesh>

{#if $gltf}
  <Trees mesh={$gltf.scene.children[0]} {count} />
{/if}
