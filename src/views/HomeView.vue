<template>
  <main class="container">
    <h1 class="text-center">Calculator Vue3 Composition API</h1>
    <div class="calculator">
      <div class="outputArea position-relative">
        <div class="position-absolute fw-bold top-10
        start-10" v-if="symbol">{{ symbol }}</div>
        <div class="p-10 text-end fs-2 fw-bold">{{ symbol === '='? Number(str)? Number(str) : total : str === '-0'? '-0' : Number(str) }}</div>
        <p class="text-end text-danger my-5 fw-bold px-10" v-if="prompt">最多 10 位數字</p>
      </div>
      <div class="buttonArea">
        <div class="d-flex justify-content-between flex-wrap">
          <div class="btn mb-10 text-center fw-bold w-210" @click="reset">Reset</div>
          <div class="btn mb-10 text-center fw-bold" @click="del">Del</div>
          <div class="btn mb-10 text-center fw-bold" @click="clickArithmetic">/</div>
          <div class="btn mb-10 text-center fw-bold" @click="storeStr">7</div>
          <div class="btn mb-10 text-center fw-bold" @click="storeStr">8</div>
          <div class="btn mb-10 text-center fw-bold" @click="storeStr">9</div>
          <div class="btn mb-10 text-center fw-bold" @click="clickArithmetic">*</div>
          <div class="btn mb-10 text-center fw-bold" @click="storeStr">4</div>
          <div class="btn mb-10 text-center fw-bold" @click="storeStr">5</div>
          <div class="btn mb-10 text-center fw-bold" @click="storeStr">6</div>
          <div class="btn mb-10 text-center fw-bold" @click="clickArithmetic">-</div>
          <div class="btn mb-10 text-center fw-bold" @click="storeStr">1</div>
          <div class="btn mb-10 text-center fw-bold" @click="storeStr">2</div>
          <div class="btn mb-10 text-center fw-bold" @click="storeStr">3</div>
          <div class="btn mb-10 text-center fw-bold" @click="clickArithmetic">+</div>
          <div class="btn mb-10 text-center fw-bold w-320" @click="storeStr">0</div>
          <div class="btn mb-10 text-center fw-bold" @click="count">=</div>
        </div>
      </div>
    </div>
  </main>
</template>
<script setup>
  import { ref } from 'vue';
  const store1 = ref(0);
  const store2 = ref(0);
  const total = ref(0);
  const str = ref('0');
  const symbol = ref('');
  const counting = ref(false);
  const prompt = ref(false);

  function storeStr(e) {
    const reg = new RegExp('^-?[\\d]{1,9}$');
    if(symbol.value === '='){
      store1.value = 0;
      store2.value = 0;
      str.value = String(total.value);
      symbol.value = '';
      counting.value = false;
    }
    reg.test(Number(str.value))? str.value += e.target.textContent : prompt.value = true;
  }
  function clickArithmetic(e) { 
    prompt.value = false;
    if(symbol.value){
      const reg = RegExp('^[+-/\\*]');
      if(Number(str.value) && symbol.value === '='){
        total.value = Number(str.value);
      }else if(reg.test(symbol.value) && counting.value){
        store1.value = Number(str.value);
        arithmetic(symbol.value);
      }else if(!counting.value && symbol.value !== '='){
        total.value = store1.value;
        store1.value = Number(str.value);
        counting.value = true;
        arithmetic(symbol.value);
      }
      symbol.value = e.target.textContent;
      str.value = '0';
    }else{
      if(counting.value){
      symbol.value = e.target.textContent;
      store2.value = Number(str.value);
      str.value = '0';
      }else{
        symbol.value = e.target.textContent;
        store1.value = Number(str.value);
        str.value = '0';
      }
    }
  }
  function count() { 
    prompt.value = false;
    if(counting.value){
      store1.value = Number(str.value);
    }else{
      if(str.value === '-0' || Number(str.value)<0){
        store1.value = Number(str.value);
        symbol.value = '-';
      }else{
        store2.value = Number(str.value);
      }
    }
    
    arithmetic();
    counting.value = true;
    str.value = '0';
    checkTotal();
  }
  function arithmetic(inputSymbol = '') {
    const regToFixedTwo = new RegExp('^-?\\d?.00$');
    const symbolSwitch = inputSymbol || symbol.value;
    inputSymbol? '' : symbol.value = '=';
    
    switch (symbolSwitch) {
      case '+':
        counting.value? total.value += store1.value : total.value = store1.value + store2.value;
        break;
      case '-':
        counting.value? total.value -= store1.value : total.value = store1.value - store2.value;
        break;
      case '*':
        counting.value? total.value *= store1.value : total.value = store1.value * store2.value;
        total.value = regToFixedTwo.test(Number((total.value).toFixed(2)))? total.value : Number((total.value).toFixed(2));
        break;
      case '/':
        if(counting.value){
          if(store1.value === 0){
            alert('除數不得為零！！！');
            reset();
            return;
          }
        }else{
          if(store2.value === 0){
            alert('除數不得為零！！！');
            reset();
            return;
          }
        }
        counting.value? total.value /= store1.value : total.value = store1.value / store2.value;
        total.value = regToFixedTwo.test(Number((total.value).toFixed(2)))? total.value : Number((total.value).toFixed(2));
        break;
      default:
        alert('操作錯誤，請重試！！！');
        reset();
        return;
    }
    store1.value = 0;
    store2.value = 0;
  }
  function checkTotal() { 
    const regInt = new RegExp('^-?[\\d]{0,10}$');
    const regFloat = new RegExp('^-?\\d{0,10}.[\\d]{0,2}$');
    
    if(String(total.value).includes('.')){
      if(!regFloat.test(total.value)){
        reset();
        alert('數值過大，無法計算！！！');
      }
    }else{
      if(!regInt.test(total.value)){
        reset();
        alert('數值過大，無法計算！！！');
      }
    }
  }
  function reset() { 
    store1.value = 0;
    store2.value = 0;
    total.value = 0;
    str.value = '0';
    symbol.value = '';
    counting.value = false;
    prompt.value = false;
  }
  function del() { 
    prompt.value = false;
    Object.is(Number(str.value.slice(0,-1)),-0)? str.value = '-0' : str.value.slice(0,-1) === '-'? str.value = '0' : str.value = str.value.slice(0,-1);
  }
</script>
<style lang="css">
.calculator{
  margin: 0 auto;
  padding: 30px;
  width: 500px;
  border: 5px solid #000;
  border-radius: 10px;
}
.outputArea{
  border: 2px solid #000;
  border-radius: 5px;
  margin-bottom: 40px;
}
.btn{
  width: 100px;
  height: 60px;
  border: 2px solid #000;
  border-radius: 5px;
  line-height: 60px;
}
.btn:hover{
  background-color: #b5b4b4;
}
.btn:active{
  background-color: #848484cd;
}
.w-210{
  width: 210px;
}
.w-320{
  width: 320px;
}
</style>
