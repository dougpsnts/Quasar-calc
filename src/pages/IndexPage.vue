<template>
  <q-page padding>
    <div class="row justify-center">
      <div class="col-12 col-md-8">
        <q-card>
          <q-card-section class="bg-primary text-white">
            <div class="text-h6">Quasar Calculator</div>
          </q-card-section>
          <q-card-section>
            <div  class="text-h5 text-grey-5 text-right">
              {{ acumulador + botaoAtual }}
            </div>
            <div class="text-h3 text-right">
              {{ resultadoFinal }}
            </div>
          </q-card-section>
          <q-card-section class="bg-grey-4">
            <div class="row q-col-gutter-sm">
              <div class="col-3" v-for="botao in botoes" :key="botao">
                <q-btn 
                class="full-width text-h6"
                :color= "notaNumber(botao) ? 'indigo' : 'grey-2'"
                :text-color="notaNumber(botao) ? 'white' : 'grey-8'"
                @click="btnAcao(botao)"
                >
                  {{ botao }}
                </q-btn>
              </div>
              <div class="col-6">
                <q-btn 
                label="Reset" 
                color="indigo" 
                class="full-width text-h6"
                @click="resetar"
                />
              </div>  
              <div class="col-6">
                <q-btn 
                label="=" 
                color="orange" 
                class="full-width text-h6" 
                @click="resultado"
                />
              </div>  
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>
    
  </q-page>
</template>

<script setup>
//
  import { ref } from 'vue'
  import { evaluate } from 'mathjs'
  const botaoAtual = ref('')
  const botoes = [7, 8, 9, '%', 4, 5, 6, '+', 1, 2, 3, '-', '.','0', '*', '/']
  const notaNumber = valor => isNaN(valor)
  const acumulador = ref('')
  const resultadoFinal = ref('')
  const operadorCalculo = ref(true)

  const btnAcao = valor => {
    if (!notaNumber(valor)) {
      if(operadorCalculo.value){
        botaoAtual.value = ''
        operadorCalculo.value = false
      }
      botaoAtual.value = `${botaoAtual.value}${valor}`
    } else {
      executarCalculo(valor)
    }
  }

  const executarCalculo = valor => {
    if(valor === '.'){
      if(botaoAtual.value.indexOf('.') === -1){
        botaoAtual.value = `${botaoAtual.value}${valor}`
      }
       return
    }  
    if(valor === '%'){
      if(botaoAtual.value !== ''){
        botaoAtual.value = `${parseFloat(botaoAtual.value) / 100}`
      }
      return
    }
    adicionarOperador(valor)
  }

  const adicionarOperador = valor => {
    if(!operadorCalculo.value){
      acumulador.value += `${botaoAtual.value} ${valor} `
      botaoAtual.value = ''
      operadorCalculo.value = true
    }
  }
    
    const resetar = () => {
      acumulador.value = ''
      botaoAtual.value = ''
      resultadoFinal.value = ''
      operadorCalculo.value = true
  }

  const resultado = () => {
    if(!operadorCalculo.value){
      resultadoFinal.value = evaluate(acumulador.value + botaoAtual.value);
    }else{
      resultadoFinal.value = 'Error!'
    }
  }
</script>
<style scoped>
  .text-h3 {
    height: 50px;
  }
  .text-h5 {
    height: 32px;
  }
</style>
