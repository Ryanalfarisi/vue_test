<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <div class="container">
      <div class="col-12 text-center">
        <div style="width:400px; margin:auto;">
         <h4>Hi, I'm front developer, Specialy on Vue.js</h4>
         <b-alert variant="danger" :show="showError">Minimal anda harus pilih dua input untuk di operasikan</b-alert>
         <div class="row my-3" v-for="i in 3" :key="i">
           <div class="col-8">
             <b-form-input
              id="input-1"
              v-model="rowNumber[i]"
              type="number"
              placeholder="Enter Number"
              required
              ></b-form-input>
           </div>
           <div class="col-4 my-auto">
            <b-form-checkbox
              :id="`checkbox-${i}`"
              v-model="checkNumber[i]"
              :name="`checkbox-${i}`">
            </b-form-checkbox>
           </div>
         </div>
         <div class="row">
           <div class="col-8 py-3" style="border-bottom: 2px solid;">
             <div class="row">
              <div class="col-3" v-for="i in operator" :key="i">
                <div class="wrapper-operator" @click="run(i)">
                  {{i}}
                </div>
              </div>
             </div>
           </div>
           <div class="col-12 my-3 text-left-important">
             <h2>Hasil: {{total}}</h2>
           </div>
         </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
/* Chrome, Safari, Edge, Opera */
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Firefox */
input[type=number] {
  -moz-appearance: textfield;
}
.wrapper-operator {
  border: 1px solid;
  border-radius: 6px;
  cursor: pointer;
}
.text-left-important {
  text-align:left
}
</style>
<script>
export default {
  name: 'App',
  data() {
    return {
      showError:false,
      operator: ["+","-","x","/"],
      total: 0,
      rowNumber: [],
      checkNumber: [],
      lowestIdx:0
    }
  },
  methods: {
    run(op) {
      this.showError = this.validateRun();
      if(this.showError) return
      this.showError = false;
      switch(op) {
        case "+":
          // code block
          this.total = 0;
          this.generateTotal(op);
          break;
        case "-":
          this.lowestIdx = this.sortingArray();
          this.total = this.rowNumber[this.lowestIdx] ? Number(this.rowNumber[this.lowestIdx]) : 0 ;
          this.generateTotal(op);
          break;
        case "x":
          this.total = 1
          this.generateTotal(op);
          break;
        case "/":
          this.lowestIdx = this.sortingArray();
          this.total = this.rowNumber[this.lowestIdx] ? Number(this.rowNumber[this.lowestIdx]) : 0 ;
          this.generateTotal(op);
          break;
        default:
      }
    },
    generateTotal(op) {
      this.checkNumber.forEach((el,id) => {
        if(el) {
          if(op == '+') {
            this.total += Number(this.rowNumber[id]);
          } else if(op == '-') {
            if(Number(id) != this.lowestIdx) {
              this.total -= Number(this.rowNumber[id])
            }
          } else if(op == 'x') {
            this.total *= Number(this.rowNumber[id])
          } else if(op  == '/') {
            if(Number(id) != this.lowestIdx) {
              console.log(this.total)
              this.total /= Number(this.rowNumber[id])
            }
          }
        }
      })
    },
    sortingArray() {
      let lowIdx;
      let kNumber;
      this.checkNumber.forEach((e,i) => {
        if(e) lowIdx = Number(i);
        this.checkNumber.forEach((j,k) => {
          if(j) kNumber = Number(k);
          if(kNumber < lowIdx) {
             lowIdx = kNumber;
          }
        })
      })
      return lowIdx;
    },
    validateRun() {
      let howMuchCheck = [];
      this.checkNumber.forEach(e => {
        if(e) howMuchCheck.push(e)
      })
      if(howMuchCheck < 2) return true
      return false
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
