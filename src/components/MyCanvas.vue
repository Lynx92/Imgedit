<template>
  <div id="myTest" class="layout">
    <v-stage class="myCanvas" ref="stage" :config="stageSize" @mousedown="handleStageMouseDown">
      <v-layer ref="layer">
        <v-image
          crossorigin="anonymous"
          ref="imageLayer"
          v-for="item in layers"
          :key="item.id"
          :config="item"
        />
        <v-transformer ref="transformer" />
      </v-layer>
    </v-stage>

    <div class="bar">
      <span>Tools</span>
      <div class="btns">
        <button @click="createRect">Create Rect</button>
        <button @click="brush">Brush</button>
        <button @click="eraser">Eraser</button>
        <!-- <button @click="download">Download</button> -->
      </div>

      <span class="lay">Layers</span>
      <ul class="layers">
        <li v-for="(layerPreview, key) in layers" :key="layerPreview.id">
          {{layerPreview.name}}
          <div class="arrows">
            <button @click="layerUp(layers,key)">&#128316</button>
            <button @click="$delete(layers,key)">🗑️</button>
            <button @click="layerDown(layers,key)">&#128317</button>
          </div>
        </li>
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
      layers: [
        {
          image: null,
          x: 100,
          y: 100,
          width: 500,
          height: 400,
          draggable: true,
          name: "Raya"
        },
        {
          image: null,
          x: 500,
          y: 200,
          width: 500,
          height: 400,
          draggable: true,
          name: "Lynx"
        }
      ],
      stageSize: {
        width: width,
        height: height
      },
      selectedShapeName: "",
      isPaint: false,
      lastLine: "",
      mode: "brush"
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
    },

    createRect(e) {
      const layer = this.$refs.layer.getStage();
      const newRect = new Konva.Rect({
        x: 0,
        y: 0,
        width: 100,
        height: 100,
        fill: "green",
        stroke: "black",
        strokeWidth: 4,
        draggable: true,
        name: "Rect"
      });

      layer.add(newRect);
      layer.draw();
    },

    brush(e) {
      const stage = this.$refs.stage.getStage();
      const layer = this.$refs.layer.getStage();
      this.mode = "brush";
      stage.on("mousedown touchstart", function(e) {
        this.isPaint = true;
        let pos = stage.getPointerPosition();
        this.lastLine = new Konva.Line({
          strokeWidth: 15,
          stroke: "red",
          globalCompositeOperation: "source-over",
          points: [pos.x, pos.y]
        });
        layer.add(this.lastLine);
      });

      stage.on("mouseup touchend", function() {
        this.isPaint = false;
      });

      stage.on("mousemove touchmove", function() {
        if (!this.isPaint) {
          return;
        }
        const pos = stage.getPointerPosition();
        let newPoints = this.lastLine.points().concat([pos.x, pos.y]);
        this.lastLine.points(newPoints);
        layer.batchDraw();
      });
    },

    eraser(e) {
      const stage = this.$refs.stage.getStage();
      const layer = this.$refs.layer.getStage();
      this.mode = "eraser";
      stage.on("mousedown touchstart", function(e) {
        this.isPaint = true;
        let pos = stage.getPointerPosition();
        this.lastLine = new Konva.Line({
          strokeWidth: 20,
          stroke: "white",
          globalCompositeOperation: "destination-out",
          points: [pos.x, pos.y]
        });
        layer.add(this.lastLine);
      });

      stage.on("mouseup touchend", function() {
        this.isPaint = false;
      });

      stage.on("mousemove touchmove", function() {
        if (!this.isPaint) {
          return;
        }
        const pos = stage.getPointerPosition();
        let newPoints = this.lastLine.points().concat([pos.x, pos.y]);
        this.lastLine.points(newPoints);
        layer.batchDraw();
      });
    },

    download() {
      const pictu = document.getElementsByTagName("canvas");
      pictu[0].setAttribute("crossOrigin", "anonymous");
      console.log(pictu[0]);
      pictu[0].toDataURL("image/png");
    }, //Can´t do because of CORS permission

    layerUp(layerPreview,key) {
      const layer = this.$refs.layer.getStage();
      layerPreview.moveUp();
      layer.draw();
    },
    layerDown(layerPreview,key) {
      const layer = this.$refs.layer.getStage();
      layerPreview.moveDown();
      layer.draw();
    }
  },
  computed: {},
  created() {
    const image = new window.Image();
    //image.setAttribute("crossOrigin", "anonymous");
    //image.crossOrigin="anonymous"
    image.origin = "anonymous";
    image.src =
      "https://www.visitlagraciosa.com/wp-content/uploads/2019/07/buceo-la-graciosa-lanzarote-raya.jpg";

    image.onload = () => {
      // set image only when it is loaded
      this.layers[0].image = image;
    };
    const image2 = new window.Image();
    image2.origin = "anonymous";
    //image2.setAttribute("crossOrigin", "anonymous");
    //image2.crossOrigin="anonymous"
    image2.src =
      "https://www.ahorasemanal.es/media/images/numero%2032/horizontal/CAP_00601417h.jpg";

    image2.onload = () => {
      // set image only when it is loaded
      this.layers[1].image = image2;
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
  border: 2px solid #df4b26;
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
  flex-direction: column-reverse;
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

.arrows {
  margin-left: 1rem;
  display: flex;
  flex-direction: column;
}
</style>