<script setup lang="ts">
  import {ref} from 'vue';
  let expressao =   ['0', '', ''];
  const opc = ref('0');

  function click(value: string): void {
    if(value == "+" || value == "-" || value == "*" || value == "/"){
      if(expressao[2] == ''){
        // if(expressao[0])
        expressao[1] = value;
        opc.value = expressao[0] + expressao[1] + expressao[2]
        // console.log(expressao)
        return
      }else{
        // console.log("expressao")
        return;
      }
    }else {
      if(expressao[1] == ''){
        if(expressao[0] == '0'){
          if(value == '.'){
            expressao[0] += value;
            opc.value = expressao[0] + expressao[1] + expressao[2]
            return;
          }
          expressao[0] = value;
          opc.value = expressao[0] + expressao[1] + expressao[2]
          return;
        }else{
          // console.log('expressao')
          if(value == '.'){
            if(expressao[0].slice(-1) == "."){
              return;
            }
          }
          expressao[0] += value;
          opc.value = expressao[0] + expressao[1] + expressao[2]
          // console.log(expressao)
          return;
        }
      }else{
        if(expressao[2] == ''){
          if(value == '.'){
            // expressao[2] += '0' + value;
            // opc.value = expressao[0] + expressao[1] + expressao[2]
            return;
          }
          expressao[2] = value;
          opc.value = expressao[0] + expressao[1] + expressao[2]
          return;
        }else{
          // console.log('expressao')
          if(value == '.'){
            if(expressao[2].slice(-1) == "."){
              return;
            }
          }
          expressao[2] += value;
          opc.value = expressao[0] + expressao[1] + expressao[2]
          // console.log(expressao)
          return;
        }
      }
    }
    
  }

  function clear():void {
    expressao[0] = '0';
    expressao[1] = '';
    expressao[2] = '';
    opc.value = expressao[0] + expressao[1] + expressao[2]
  }

  function clearlatest():void {
    if(expressao[2] != '' && expressao[0] != '0' && expressao[1] != ''){
      let str = expressao[2]
      str = str.slice(0, -1);
      expressao[2] = str;
      opc.value = expressao[0] + expressao[1] + expressao[2]
      return;
    }else if(expressao[1] != '' && expressao[0] != '0' && expressao[2] == ''){
      let str = expressao[1]
      str = str.slice(0, -1);
      expressao[1] = str;
      opc.value = expressao[0] + expressao[1] + expressao[2]
      return;
    }else{
      let str = expressao[0]
      if(str.length <= 1){
        str = '0';
        expressao[0] = str;
        opc.value = expressao[0] + expressao[1] + expressao[2]
        return;
      }
      str = str.slice(0, -1);
      expressao[0] = str;
      opc.value = expressao[0] + expressao[1] + expressao[2]
      return;
    }
    // if(opc.value.length == 1){
    //   opc.value = '0';
    //   return
    // }
    // let str = opc.value;
    // str = str.slice(0, -1); // Remove a última letra
    // opc.value = str;
    // // console.log(str)
  }

function calcular(): number {
  let sum = 0
  if(expressao[1] == "+") {
    sum = parseFloat(expressao[0]) + parseFloat(expressao[2]);
  }else if(expressao[1] == "-"){
    sum = parseFloat(expressao[0]) - parseFloat(expressao[2]);
  }else if(expressao[1] == "*"){
    sum = parseFloat(expressao[0]) * parseFloat(expressao[2]);
  }else{
    sum = parseFloat(expressao[0]) / parseFloat(expressao[2]);
  }
  return sum;
}

function soma(): void {
  if(expressao[2] == '') {
    return;
  };
  const resultado = calcular().toFixed(2);
  opc.value = resultado.toString(); // Atualiza o visor com o resultado
  expressao[0] = resultado.toString();
  expressao[1] = '';
  expressao[2] = '';
}
  
</script>

<template>
  <div class="body">
    <div class="calculadora">
      <div class="visor">
        <input type="text" v-model="opc" readonly>
      </div>
      <div class="container">
        <div class="one flex">
          <button @click="clear" class="btn" style="background-color: #3498db;">AC</button>
          <button @click="clearlatest" class="btn" style="background-color: #e74c3c;">C</button>
          <button @click="click('/')" class="btn" style="background-color: #4caf50;">÷</button>
          <button @click="click('*')" class="btn" style="background-color: #4caf50;">x</button>
        </div>
        <div class="two flex">
          <button @click="click('7')" class="btn btnpadrao">7</button>
          <button @click="click('8')" class="btn btnpadrao">8</button>
          <button @click="click('9')" class="btn btnpadrao">9</button>
          <button @click="click('-')" class="btn" style="background-color: #4caf50;">-</button>
        </div>
        <div class="three flex">
          <button @click="click('4')" class="btn btnpadrao">4</button>
          <button @click="click('5')" class="btn btnpadrao">5</button>
          <button @click="click('6')" class="btn btnpadrao">6</button>
          <button @click="click('+')" class="btn btnpadrao" style="background-color: #4caf50;">+</button>
        </div>
        <div class="four flex">
          <button @click="click('1')" class="btn btnpadrao">1</button>
          <button @click="click('2')" class="btn btnpadrao">2</button>
          <button @click="click('3')" class="btn btnpadrao">3</button>
          <button @click="soma" class="btn btnpadrao" style="background-color: #e67e22">=</button>
        </div>
        <div class="five flex">
          <button @click="click('0')" class="btn btnpadrao" style="width: 90%; font-size: 1.6rem;">0</button>
          <button @click="click('.')" class="btn btnpadrao">.</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

  /* .five {
    gap: 1rem;
  } */

  .body {
    display: flex;
    width: 100%;
    height: 100vh;
    justify-content: center;
    align-items: flex-start;
    padding-top: 1.6rem;
  }

  .calculadora {
    /* margin: 2rem auto; */
    padding: 2rem;
    background-color: #333333;
    width: 335px; 
    height: 394px;
    border-radius: 1rem;
  }

  .visor {
    display: flex;
    justify-content: flex-end;
    align-items: flex-end;
    padding: 0 1.6rem 1rem 0;
    background-color: #444444;
    width: 100%;  
    height: 50px;
    border-radius: 10px;
    margin-bottom: 2rem;
  }

  .visor input {
    height: 2rem;
    width: 169px;
    border-radius: 3px;
    font-size: 1.6rem;
  }

  .container {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  .flex {
    /* margin-top: 3px; */
    display: flex;
    justify-content: space-between;
    gap: 1rem;
  }

  .btn {
    cursor: pointer;
    border-radius: 10px;
    color: #ffffff;
    font-size: 2rem;
    width: 7.975rem;
    height: 4.9rem;
  }

  .btnpadrao {
    background-color: #555555;
  }

  @media screen and (max-width: 320px) {
    .body {
      /* padding: 1.6rem; */
      justify-content: flex-start; /* Alinha os itens ao topo */
      align-items: flex-start; /* Alinha os itens à esquerda */
    }
  }
</style>
