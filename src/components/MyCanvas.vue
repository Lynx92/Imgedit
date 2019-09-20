<template>
  <div class="layout">
    <v-stage class="myCanvas" ref="stage" :config="stageSize" @mousedown="handleStageMouseDown">
      <v-layer ref="layer">
        <v-rect v-for="item in rectangles" :key="item.id" :config="item" />
        <v-transformer ref="transformer" />
      </v-layer>
      <v-layer ref="layer">
        <v-regular-polygon
          @mousemove="handleMouseMove"
          @mouseout="handleMouseOut"
          :config="{
          x: 80,
          y: 120,
          sides: 3,
          radius: 80,
          fill: '#00D2FF',
          stroke: 'black',
          strokeWidth: 4
        }"
        />
        <v-text
          ref="text"
          :config="{
          x: 10,
          y: 10,
          fontFamily: 'Calibri',
          fontSize: 24,
          text: '',
          fill: 'black'
        }"
        />
      </v-layer>
    </v-stage>
    <div class="bar">
      <span>Tools</span>
      <div class="btns">
        <button>Drag & Rotate</button>
        <button>ADD</button>
        <button>Add</button>
        <button id="addLayer" @click="addLayer">Add Layer</button>
      </div>

      <span class="lay">Layers</span>
      <div class="layers">
        <div>1</div>
      </div>
    </div>
  </div>
</template>

<script>
const width = window.innerWidth / 1.2;
const height = window.innerHeight / 1.2;

export default {
  name: "MyCanvas",

  data() {
    return {
      layers: ["layer"],
      layerCounter: 1,

      stageSize: {
        width: width,
        height: height
      },
      rectangles: [
        {
          x: 10,
          y: 10,
          width: 100,
          height: 100,
          fill: "red",
          name: "rect1",
          draggable: true
        },
        {
          x: 150,
          y: 150,
          width: 100,
          height: 100,
          fill: "green",
          name: "rect2",
          draggable: true
        }
      ],
      selectedShapeName: ""
    };
  },
  methods: {
    handleStageMouseDown(e) {
      // clicked on stage - cler selection
      if (e.target === e.target.getStage()) {
        this.selectedShapeName = "";
        this.updateTransformer();
        return;
      }

      // clicked on transformer - do nothing
      const clickedOnTransformer =
        e.target.getParent().className === "Transformer";
      if (clickedOnTransformer) {
        return;
      }

      // find clicked rect by its name
      const name = e.target.name();
      const rect = this.rectangles.find(r => r.name === name);
      if (rect) {
        this.selectedShapeName = name;
      } else {
        this.selectedShapeName = "";
      }
      this.updateTransformer();
    },

    updateTransformer() {
      // here we need to manually attach or detach Transformer node
      const transformerNode = this.$refs.transformer.getStage();
      const stage = transformerNode.getStage();
      const { selectedShapeName } = this;

      const selectedNode = stage.findOne("." + selectedShapeName);
      // do nothing if selected node is already attached
      if (selectedNode === transformerNode.node()) {
        return;
      }

      if (selectedNode) {
        // attach to another node
        transformerNode.attachTo(selectedNode);
      } else {
        // remove transformer
        transformerNode.detach();
      }
      transformerNode.getLayer().batchDraw();
    },

    addLayer() {
      if (this.layerCounter < 4) {
        this.layers.push("layer");
        this.layerCounter++;
        const layerSelector = document.querySelector(".layers");
        const divNode = document.createElement("div");
        const layerNode = document.createTextNode(this.layerCounter);
        divNode.appendChild(layerNode);
        layerSelector.appendChild(divNode);
      }
        console.log(this.layers);
        console.log(this.layers.length);
    }
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
  margin-top: 1rem;
  display: flex;
  flex-wrap: wrap;
  div {
    display: flex;
    border: 1px solid blue;
    margin: 0 0 1rem 1rem;
    width: 8vw;
    height: 8vh;
    align-items: center;
    justify-content: center;
  }
}

.myCanvas {
  background: #f0f0f0;
}
</style>