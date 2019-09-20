<template>
  <div class="layout">
    <v-stage class="myCanvas" ref="stage" :config="stageSize" @mousedown="handleStageMouseDown">
      <v-layer ref="layer">
        <v-image v-for="item in layers" :key="item.id" :config="item" />
        <v-transformer ref="transformer" />
      </v-layer>
    </v-stage>

    <div class="bar">
      <span>Tools</span>
      <div class="btns">
        <button>A...</button>
        <button>B...</button>
        <!-- <button @click="addLayer">Duplicate Layer</button> -->
        <!-- <button @click="removeLayer(index)">Remove Layer</button> -->
      </div>

      <span class="lay">Layers</span>
      <ul class="layers">
        <li v-for="layerPreview in layers" :key="layerPreview.id">{{layerPreview.id}}</li>
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
      layerCounter: 1,
      layers: [
        {
          image: null,
          x: 100,
          y: 100,
          width: 300,
          height: 300,
          draggable: true,
          name: "img1"
        }
      ],
      stageSize: {
        width: width,
        height: height
      },
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
      const clickedOnTransformer =
        e.target.getParent().className === "Transformer";
      if (clickedOnTransformer) {
        return;
      }

      // find clicked rect by its name
      const name = e.target.name();
      const rect = this.layers.find(r => r.name === name);
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
    }

    // addLayer() {
    //   // Añadir Capas
    //   if (this.layers.length < 4) {
    //     if (this.layers.indexOf(this.layerCounter) == -1) {
    //       this.layers.push(this.layerCounter);
    //     } else {
    //       this.layerCounter++;
    //       this.layers.push(this.layerCounter);
    //     }
    //   }
    // }
  },
  computed: {
    // selectLayer(elem) {
    //   this.selectedLayer = true;
    // },
    // removeLayer(index) {
    //   if (this.layers.length > 0) {
    //     this.layers.splice(index, 1);
    //     this.layerCounter--;
    //   }
    // }
  },
  created() {
    const image = new window.Image();
    image.src =
      "https://www.visitlagraciosa.com/wp-content/uploads/2019/07/buceo-la-graciosa-lanzarote-raya.jpg";
    image.onload = () => {
      // set image only when it is loaded
      this.layers[0].image = image;
    };
  }
};
</script>




<style scoped lang="scss">
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