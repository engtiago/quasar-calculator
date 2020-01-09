<template>
  <div class="full-width row justify-center items-start content-start">
    <div class="q-pa-md">
      <div class="row">
        <div class="col text-center tela bg-dark">{{current || 0}}</div>
      </div>
      <div class="row">
        <div @click="clearAll" class="col btn-calc bg-grey-7">AC</div>
        <div @click="clear" class="col btn-calc bg-grey-7">C</div>
        <div @click="sign" class="col btn-calc bg-grey-7">+/-</div>
        <div  class="col btn-calc bg-dark">🤖</div>
      </div>
      <div class="row">
        <div @click="append('7')" class="col btn-calc bg-grey-9">7</div>
        <div @click="append('8')" class="col btn-calc bg-grey-9">8</div>
        <div @click="append('9')" class="col btn-calc bg-grey-9">9</div>
        <div @click="divide" class="col btn-calc bg-deep-orange-7">÷</div>
      </div>
      <div class="row">
        <div @click="append('4')" class="col btn-calc bg-grey-9">4</div>
        <div @click="append('5')" class="col btn-calc bg-grey-9">5</div>
        <div @click="append('6')" class="col btn-calc bg-grey-9">6</div>
        <div @click="minus" class="col btn-calc bg-deep-orange-7">-</div>
      </div>
      <div class="row">
        <div @click="append('1')" class="col btn-calc bg-grey-9">1</div>
        <div @click="append('2')" class="col btn-calc bg-grey-9">2</div>
        <div @click="append('3')" class="col btn-calc bg-grey-9">3</div>
        <div @click="plus" class="col btn-calc bg-deep-orange-7">+</div>
      </div>
      <div class="row">
        <div @click="append('0')" class="col btn-calc bg-grey-9">0</div>
        <div @click="dot()" class="col-4 btn-calc bg-grey-9">.</div>
        <div @click="equal()" class="col-4 btn-calc bg-deep-orange-7">=</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      previous: null,
      current: '',
      operator: null,
      operatorClicked: false
    }
  },
  methods: {
    clear () {
      this.current = ''
    },
    clearAll () {
      this.current = ''
      this.previous = null
      this.operatorvalue = ''
    },
    sign () {
      if (this.current !== '') {
        this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`
      }
    },
    append (number) {
      this.current = this.current.length < 8 && !(number === '0' && this.current === '') ? `${this.current}${number}` : this.current
    },
    dot () {
      if (this.current.indexOf('.') === -1 && this.current !== '') {
        this.append('.')
      }
    },
    setPrevious () {
      this.previous = this.current
      this.operatorClicked = true
      this.current = ''
    },
    divide () {
      this.operator = (a, b) => a / b
      this.setPrevious()
    },
    // times () {
    //   this.operator = (a, b) => a * b
    //   this.setPrevious()
    // },
    minus () {
      this.operator = (a, b) => a - b
      this.setPrevious()
    },
    plus () {
      this.operator = (a, b) => a + b
      this.setPrevious()
    },
    equal () {
      const validacao = !isNaN(parseFloat(this.previous)) && isFinite(this.previous)
      if (this.operatorClicked && validacao) {
        const res = `${this.operator(
          parseFloat(this.current),
          parseFloat(this.previous)
        )}`
        if (res.length < 8) {
          this.current = res
        } else if (isFinite(res)) {
          this.current = parseFloat(res).toFixed(3)
        } else {
          this.current = 'ERR'
        }
        this.previous = null
        this.operatorClicked = false
      }
    }
  }
}
</script>

<style>
.tela {
  color: white;
  height: 60px;
  line-height: 60px;
  text-align: center;
  font-size: 30px;
}
.btn-calc {
  color: white;
  height: 60px !important;
  width: 60px !important;
  border: 1px solid #999;
  text-align: center;
  line-height: 60px;
  margin: 2px;
  font-size: 24px;
  transition-duration: .2s;
}

.btn-calc:active{
  transform: scale(1.1);
}

.operator{
  font-size: 15px;
}

</style>
