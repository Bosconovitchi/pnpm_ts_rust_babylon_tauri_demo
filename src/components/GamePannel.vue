<script setup lang="ts">
  import {Engine, Scene, FreeCamera, Vector3, HemisphericLight, Mesh} from "babylonjs";
  import {onMounted,ref} from "vue";
  const gameWindow = ref();

  onMounted(() => {

    const canvas = gameWindow.value;
    const engine = new Engine(canvas,true, {preserveDrawingBuffer: true, stencil: true});
    console.log('gameWindow',canvas)
    function createScene () : Scene {
      const scene = new Scene(engine);
      const camera = new FreeCamera('mainCamera', new Vector3(0,5,-10),scene);
      camera.setTarget(Vector3.Zero());
      camera.attachControl(canvas,false);
      const light = new HemisphericLight('mainLight',new Vector3(0,1,0),scene);
      const sphere = Mesh.CreateSphere('sphere',16,2,scene,false,Mesh.FRONTSIDE);
      sphere.position.y = 1;
      Mesh.CreateGround('ground',6,6,2,scene,false)
      return scene;
    }
    const scene = createScene();

    engine.runRenderLoop(() => {
      scene.render()
    })
    window.addEventListener('resize',() => {
      engine.resize()
    })
  })
</script>

<template>
  <canvas class="game-pannel" ref="gameWindow"/>
</template>

<style lang="sass" scoped>
.game-pannel
  width: 100%
  height: 100%
</style>
