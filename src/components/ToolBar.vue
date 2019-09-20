<template>
  <div class="bar">

    <span>Tools</span>
    <div class="btns">
      <button>Add</button>
      <button>Add</button>
      <button>Add</button>
      <button>Add</button>
    </div>

    <span class="lay">Layers</span>
    <div class="layers">
      <div>1</div>
      <div>2</div>
      <div>3</div>
      <div>4</div>
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

</style>