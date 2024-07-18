<script lang="ts">
  import { T } from "@threlte/core";
  import { OrbitControls, useGltf } from "@threlte/extras";

  import Environment from "./Environment.svelte";
  import Trees from "./Trees.svelte";

  const count = 1000000;
  const terrainSize = 125000;

  const gltf: any = useGltf("/tree.glb");
</script>

<T.PerspectiveCamera makeDefault position={[0, 100, 0]} fov={70} far={3000}>
  <OrbitControls target.y={1.5} target.z={3} />
</T.PerspectiveCamera>

<Environment />

<T.Mesh receiveShadow rotation.x={Math.PI / -2}>
  <T.PlaneGeometry args={[terrainSize, terrainSize, 10, 10]} />
  <T.MeshLambertMaterial color={0x004622} />
</T.Mesh>

{#if $gltf}
  <Trees mesh={$gltf.scene.children[0]} {count} {terrainSize} />
{/if}
