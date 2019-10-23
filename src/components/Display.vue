<template>
  <div class="display">
    {{value}} <!-- interpolando o value -->
  </div>
</template>

<script>
export default {
  //paramêtro é em vetor para pegar os 2 elementos
  props: ['value']
}
</script>

<style>
.display {
  grid-column: span 4;
  background-color: black;
  display:flex;
  justify-content: flex-end;
  align-items: center;
  padding: 20px;
  font-size: 2.2em;
  overflow: hidden;
}
</style>
