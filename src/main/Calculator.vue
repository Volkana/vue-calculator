<template>
  <div class="calculator">
    <Display :value="displayValue"/>
    <Button label="AC" triple @onClick="clearMemory"/>
    <Button label="/" operation @onClick="setOperation"/>
    <Button label="7" @onClick="addDigit"/>
    <Button label="8" @onClick="addDigit"/>
    <Button label="9" @onClick="addDigit"/>
    <Button label="*" operation @onClick="setOperation"/>
    <Button label="4" @onClick="addDigit"/>
    <Button label="5" @onClick="addDigit"/>
    <Button label="6" @onClick="addDigit"/>
    <Button label="-" operation @onClick="setOperation"/>
    <Button label="1" @onClick="addDigit"/>
    <Button label="2" @onClick="addDigit"/>
    <Button label="3" @onClick="addDigit"/>
    <Button label="+" operation @onClick="setOperation"/>
    <Button label="0" double @onClick="addDigit"/>
    <Button label="." @onClick="addDigit"/>
    <Button label="=" operation @onClick="setOperation"/>
  </div>
</template>

<script>
import Display from "../components/Display.vue"
import Button from "../components/Button.vue"

export default {
  //criar atributo data com função para representar o estado inicial do objeto
  //criar situação para a limpeza do display
  //criar atributo para a operation inicialmente nula
  //vetor para representar os valores
  //current - valor a ser trabalhado
  data: function() {
      return {
        displayValue: "0",
        clearDisplay: false,
        operation: null,
        values: [0, 0],
        current: 0
    }
  },
  components: { Button, Display },
  //criando métodos dos botões
  methods: {
    clearMemory(){
      //limpar para voltar ao estado inicial
      Object.assign(this.$data, this.$options.data())
    },
    setOperation(operation){
      //se o usuário mexe no primeiro elemento, coloca a operação recebida
      //passa a mexer com o segundo valor
      //próximo dígito limpa o display
      if (this.current === 0){
        this.operation = operation 
        this.current = 1
        this.clearDisplay = true
      } else {
        //variável para guardar a operação quando for finalizada
        //define qual é a operação atual
        //interpola o índice 0, a operação e depois o índice 1
        const equals = operation === "="
        const currentOperation = this.operation

        try {
          this.values[0] = eval(
            `${this.values[0]} ${currentOperation} ${this.values[1]}`
          )
        } catch (e) {
          this.$emit('onError', e)
        }
        //zera o segundo valor para outra operação 
        this.values[1] = 0
        //resultado da operação no outro display
        this.displayValue = this.values[0]
        //prepara para a operação
        this.operation = equals ? null : operation
        //se for equals vai pro 0, se não vai pro 1
        this.current = equals ? 0 : 1
        //limpa o display se for diferente de equals
        this.clearDisplay = !equals
      }
    },
    addDigit(n){
      //validar o ponto
      if (n=== "." && this.displayValue.includes(".")){
        return
      }
      //limpa o display quando é 0 ou quando clearDisplay for true
      const clearDisplay = this.displayValue === "0" || this.clearDisplay
      //limpar o display com o currentValue vazio ou seta pelo displayValue
      const currentValue = clearDisplay ? "" : this.displayValue
      //a partir do displayValue concatena o currentValue com o novo valor
      const displayValue = currentValue + n
      
      //muda o estado do displayValue
      this.displayValue = displayValue
      //clearDisplay assim que o digita novo valor
      this.clearDisplay = false

      //colocando valores no vetor
      if (n !== ".") {
        const i = this.current
        const newValue = parseFloat(displayValue)
        this.values[i] = newValue
      }
    }
  }
}
</script>

<style>
.calculator {
  height: 320px;
  width: 235px;
  border-radius: 5px;
  overflow: hidden;
  display: grid;
  grid-template-columns: repeat(4, 25%);
  grid-template-row: 1fr 48px 48px 48px 48px 48px;
}
</style>
