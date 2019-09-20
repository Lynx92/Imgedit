<template>
  <div class="layout">
    <v-stage class="myCanvas" ref="stage" :config="stageSize">
      <v-layer ref="pepe" v-for="layer in layers" :key="layer.id">
       
        <v-circle
          @click="selectLayer(layer)"
          :config="{
        x: 100,
        y: 100,
    radius: 80,
    fill: 'red',
    draggable: true,
        }"
        ></v-circle>
      </v-layer>

    </v-stage>

    <div class="bar">
      <span>Tools</span>
      <div class="btns">
        <button>A...</button>
        <button>B...</button>
        <button @click="addLayer">Duplicate Layer</button>
        <button @click="removeLayer(index)">Remove Layer</button>
      </div>

      <span class="lay">Layers</span>
      <ul class="layers">
          <!-- v-bind:class="{layerActive: layers[0].selectedLayer}" -->
        <li
          v-for="layerPreview in layers"
          :key="layerPreview.id"
        >{{layerPreview.id}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
const width = window.innerWidth / 1.2;
const height = window.innerHeight / 1.2;

export default {
  data() {
    return {
      layers: [1],
      layerCounter: 1,
      stageSize: {
        width: width,
        height: height
      }
    };
  },
  methods: {
    addLayer() {
      // Añadir Capas
      if (this.layers.length < 4) {
        if (this.layers.indexOf(this.layerCounter) == -1) {
          this.layers.push(this.layerCounter);
        } else {
          this.layerCounter++;
          this.layers.push(this.layerCounter);
        }
      }
    }
  },
  computed: {
    selectLayer(elem) {
      this.selectedLayer = true;
    },
    removeLayer(index) {
      if (this.layers.length > 0) {
        this.layers.splice(index, 1);
        this.layerCounter--;
      }
    },
    
  }
};
</script>




<style lang="scss">
.layout {
  display: flex;
  justify-content: center;
  align-items: center;
}

.myCanvas {
  border: 2px solid black;
}

.bar {
  border: 2px solid red;
  margin-left: 1rem;
  width: 20vw;
  padding: 1rem;
  text-align: center;
}

.btns {
  display: flex;
  flex-direction: column;

  button {
    margin-top: 1rem;
    padding: 1rem;
    &:last-of-type {
      margin-bottom: 2rem;
    }
  }
}

.layers {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  list-style-type: none;
  align-items: center;
  justify-content: center;
  padding-left: 0;

  li {
    display: flex;
    border: 1px solid blue;
    width: 8vw;
    height: 8vh;
    align-items: center;
    justify-content: center;
    margin: 1rem;
  }
}

.myCanvas {
  background: #f0f0f0;
}

.layerActive {
  background: rgba(0, 0, 255, 0.4);
}
</style>