<template>
  <div class="bar">
    ToolBar
    <div class="btns">
      <button>Add</button>
      <button>Add</button>
      <button>Add</button>
      <button>Add</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ToolBar",
  props: {}
};
</script>

<style lang="scss">
.bar {
  border: 2px solid red;
}

.btns {
  display: flex;
  flex-direction: column;
  button {
    margin-top: 1rem;
  }
}
</style>