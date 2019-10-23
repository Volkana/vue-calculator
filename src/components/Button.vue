<template>
  <!-- :class="" é a condicional para verificar o tipo do botão-->
  <!-- @click dispara o evento criado com o $emit e o label como parâmetro -->
  <button class="button" @click="$emit('onClick',label)" :class="{double, triple, operation}">
    {{label}} <!--interpolando o label do botão-->
  </button>
</template>

<script>
export default {
  //definindo props - parâmetros - tipos dos botões
  props: {
    label: {},
    operation: {type: Boolean},
    double: {type: Boolean},
    triple: {type: Boolean}
  }
}
</script>

<style>
/*criando uma pseudo-classe para herdar variáveis em outras*/
:root {
  --bg-button: lightgray;
  --border-button: solid 1px grey;
}

.button {
  font-size: 1.4em;
  background-color: var(--bg-button);
  border:none;
  border-right: var(--border-button);
  border-bottom: var(--border-button);
  outline: none;
}

.button:active {
  background-color: darkgray;
}

.button.double {
  grid-column: span 2;
}

.button.triple {
  grid-column: span 3;
}

.button.operation {
  background-color: orange;
  color: white;
}

.button.operation:active {
  background-color: orangered;
}
</style>
