<template>
  <div id="app">
    <div class="container">
      <header id="title">
         <h1>Nano Converter</h1>
      </header>
      <main>
         <section id="number-system">
            <h2>Number System Conversion</h2>
        
            <div class="box">
               <div class="input-group">
                  <label for="">Decimal</label>
                  <input type="text" name="" id="" placeholder="Input Any Decimal Number" v-model="numbers.decimal">
               </div>
               <div class="input-group">
                  <label for="">Binary</label>
                  <input type="text" name="" id="" placeholder="Input Any Binary Number" v-model="numbers.binary">
               </div>
               <div class="input-group">
                  <label for="">Octal</label>
                  <input type="text" name="" id="" placeholder="Input Any Octal Number" v-model="numbers.octal">
               </div>
               <div class="input-group">
                  <label for="">Hexadecimal</label>
                  <input type="text" name="" id="" placeholder="Input Any Hexadecimal Number" v-model="numbers.hexadecimal">
               </div>
               <div class="reset-btn-container">
                 <p v-if="invalidNumber" style="color:red">Invalid Input</p>
                  <button @click="resetNumbers">Reset</button>
               </div>
               <div class="one-rem-space"></div>       
            </div>
         </section>

         <section id="text-conversion">
            <h2>Text Conversion</h2>
            <div class="box">
               <div class="input-group">
                  <label>Enter Your Text Below</label>
                  <textarea 
                     name="text-field" id="" cols="30" rows="10" v-model="lines.text">
                  </textarea>
               </div>
               <div class="half-rem-space"></div>
               <div class="input-group">
                  <label>Enter Your Binary Below</label>
                  <textarea 
                     name="binary-field" id="" cols="30" rows="10" v-model="lines.binary">
                  </textarea>
               </div>
               <div class="reset-btn-container">
                  <button @click="resetLines">Reset</button>
               </div>
               <div class="one-rem-space"></div>
            </div>
         </section>
      </main>
   </div>
  </div>
</template>

<script>
require('@/assets/reset.css');
require('@/assets/fonts.css');
require('@/assets/style.css');

function isBinary(text){
  for (let t of text){
    if (t != '0' && t != '1') return false
  }
  return true
}

function isOctal(text){
  for (let t of text){
    if (t < '0' || t > '7') return false
  }
  return true
}

function isHex(text){
  for (let t of text) {
    if(
      (t < '0' || t > '9') &&
      (t < 'A' || t > 'F') &&
      (t < 'a' || t > 'f')
    ){
      return false;
    }
  }
  return true;
}

function textToBinary(text){
  const charCodeArray = []
  for (let i in text){
    charCodeArray.push(text.charCodeAt(i))
  }
  const binaryArray =  charCodeArray.map(char => {
    return char.toString(2)
  })
  return binaryArray;
}

function binaryToText(codeArray){
  let text = ''
  for (let code of codeArray){
    const  char = String.fromCharCode(parseInt(code, 2))
    text = text.concat(char)

  }
  return text;
}

export default {
  name: 'App',
  created(){
    document.title = "Nano Converter";
  },
  data(){
    return{
      numbers: {
        decimal: 0,
        binary: 0,
        octal: 0,
        hexadecimal: 0,
      },
      lines: {
        text: '',
        binary: '',

      },
      invalidNumber: false,
      invalidLine: false
    };
  },
  methods: {
    resetNumbers(){
      this.numbers = {
        decimal: 0,
        binary: 0,
        octal: 0,
        hexadecimal: 0,
      },
    
      this.invalidNumber = false;
    },
    resetLines(){
      this.lines = {
        text: '',
        binary: '',
  
      },
    
      this.invalidNumber = false;
    }
  },
  watch: {
    'numbers.decimal': function(value){
      this.numbers.decimal = parseInt(value) || 0;
      this.numbers.binary = value.toString(2);
      this.numbers.octal = value.toString(8);
      this.numbers.hexadecimal = value.toString(16);
    },
    'numbers.binary': function(value){
      let decimal = parseInt(value, 2)
      if (!isBinary(value)){
        this.invalidNumber = true;
      } else{
        this.invalidNumber = false;
      }

      this.numbers.decimal = decimal;
      this.numbers.binary = value || 0;
      this.numbers.octal = decimal.toString(8);
      this.numbers.hexadecimal = decimal.toString(16);
    },
    'numbers.octal': function(value){
      let decimal = parseInt(`0${value}`, 8) || 0

      if (!isOctal(value)){
        this.invalidNumber = true;
      } else{
        this.invalidNumber = false;
      }

      this.numbers.decimal = decimal;
      this.numbers.binary = decimal.toString(2) || 0;
      this.numbers.octal = value || 0;
      this.numbers.hexadecimal = decimal.toString(16);
    },
    'numbers.hexadecimal': function(value){
      let decimal = parseInt(`0x${value}`, 16) || 0

      if (!isHex(value)){
        this.invalidNumber = true;
      } else{
        this.invalidNumber = false;
      }

      this.numbers.decimal = decimal;
      this.numbers.binary = decimal.toString(2) || 0;
      this.numbers.octal = decimal.toString(8);
      this.numbers.hexadecimal = value || 0;
    },
    'lines.text': function(value){
      if (value.length == 0){
        this.lines.binary = ''
      } else{
        const codeArray = textToBinary(value)
        this.lines.binary = codeArray.join(' ');
      }
    },
    'lines.binary': function(value){
      if (value.length == 0){
        this.lines.text = ''
      } else{
        const codeArray = value.split(' ');
        const text = binaryToText(codeArray)
        this.lines.text = text;
      }
    },

  },
  
};
</script>

