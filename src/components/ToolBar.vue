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
  }
}

</style>