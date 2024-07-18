<script lang="ts">
  import { useTask, useThrelte, T } from "@threlte/core";
  import { AmbientLight, DirectionalLight, FogExp2, Vector3 } from "three";
  import { Sky } from "three/examples/jsm/objects/Sky";

  const sun = new Vector3();
  const sky = new Sky();
  sky.scale.setScalar(450000);
  const uniforms = sky.material.uniforms;
  uniforms["turbidity"].value = 5;
  uniforms["rayleigh"].value = 2;

  const { scene, camera } = useThrelte();

  scene.fog = new FogExp2("white", 0.0004);
  const sunOffset = new Vector3();

  let light: DirectionalLight;

  useTask(() => {
    sun.setFromSphericalCoords(
      1,
      Math.PI / -1.9 + performance.now() * 0.0005,
      Math.PI / 1.4
    );
    uniforms["sunPosition"].value.copy(sun);

    sunOffset.copy(sun).multiplyScalar(1000);

    light.intensity = sun.y > 0.05 ? 10 : Math.max(0, (sun.y / 0.05) * 10);
    light.position.copy($camera.position).add(sunOffset);
    light.target.position.copy($camera.position).sub(sunOffset);

    if (!scene.fog) return;
    scene.fog.color.setHSL(0, 0, sun.y);
  });

  $: {
    if (light) {
      light.shadow.mapSize.set(1024, 1024);
      light.shadow.camera.left = -1500;
      light.shadow.camera.right = 1500;
      light.shadow.camera.top = 1500;
      light.shadow.camera.bottom = -1500;
      light.shadow.camera.far = 5000;
      light.shadow.camera.updateProjectionMatrix();
    }
  }
</script>

<T is={sky} />

<T.DirectionalLight bind:ref={light} castShadow />
<T.AmbientLight />
