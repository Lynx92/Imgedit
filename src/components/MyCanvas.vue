<template>
  <v-stage class="myCanvas" ref="stage" :config="configKonva">
    <v-layer ref="layer">
      <v-circle :config="configCircle"></v-circle>
    </v-layer>
  </v-stage>
</template>

<script>


const width = window.innerWidth / 1.5;
const height = window.innerHeight / 1.5;


export default {
  name: "MyCanvas",
  props: {},

  data() {
    return {
      configKonva: {
        width: width,
        height: height
      },
      configCircle: {
        x: 100,
        y: 100,
        radius: 70,
        fill: "red",
        stroke: "black",
        strokeWidth: 4
      }
    };
  }
};
</script>

<style lang="scss">
.myCanvas {
  border: 2px solid black;
}
</style>