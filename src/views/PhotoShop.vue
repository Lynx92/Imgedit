<template>
  <div>
    <h1>Vista PhotoShop</h1>
    <div class="layout">
      <MyCanvas />
      <ToolBar />
    </div>
  </div>
</template>

<script>
import MyCanvas from "@/components/MyCanvas.vue";
import ToolBar from "@/components/ToolBar.vue";

export default {
  name: "photoShop",
  components: {
    MyCanvas,
    ToolBar
  }
};
</script>

<style lang="scss">
h1 {
  text-align: center;
}

.layout {
    display: flex;
    justify-content: center;
}
</style>