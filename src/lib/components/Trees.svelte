<script lang="ts">
  import { CullingBVH, InstancedMesh2 } from "@three.ez/instanced-mesh-test";
  import { useThrelte } from "@threlte/core";
  import { Mesh, BufferGeometry, MeshStandardMaterial } from "three";
  import { T } from "@threlte/core";

  export let mesh: Mesh<BufferGeometry, MeshStandardMaterial>;
  export let count: number;
  export let terrainSize: number;

  const { renderer } = useThrelte();

  const trees = new InstancedMesh2(renderer, count, {
    cullingType: CullingBVH,
    geometry: mesh.geometry,
    material: mesh.material,
    onInstanceCreation: (obj, index) => {
      obj.position
        .setX(Math.random() * terrainSize - terrainSize / 2)
        .setZ(Math.random() * terrainSize - terrainSize / 2);
      obj.scale.setScalar(Math.random() * 0.1 + 0.1);
      obj
        .rotateY(Math.random() * Math.PI * 2)
        .rotateZ(Math.random() * 0.3 - 0.15);
    },
  });

  trees.castShadow = true;
</script>

<T is={trees} />
