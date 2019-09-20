<template>
  <div>
    <h1>Vista PhotoShop</h1>
    <div>
      <MyCanvas />
    </div>
  </div>
</template>

<script>
import MyCanvas from "@/components/MyCanvas.vue";

export default {
  name: "photoShop",
  components: {
    MyCanvas,
    
  }
};
</script>

<style lang="scss">
h1 {
  text-align: center;
}
</style>