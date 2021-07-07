<template>
  <button>
    <slot>{{title}}</slot>
  </button>
</template>
<script>
export default {
  props:["title"]
}
</script>
<style scoped>
button {
  font: inherit;
  border: 1px solid #000000;
  background-color: #00fa70;
  color: white;
  padding: 0.5rem 2rem;
  cursor: pointer;
  border-radius: 5px;
  transition: .3s linear;
  font-weight: bold;
  color: #000;
}

button:hover,
button:active {
  background-color: #5c0556;
  border-color: #5c0556;
  color: #fff;
}
</style>