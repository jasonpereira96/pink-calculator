<template>
  <b-container class="main-container">
    <b-row class="main-row">
      <b-col cols="12" class="calculator-wrapper">
        <div class='calculator'>
          <div class="display">
            <b-container>
              <b-row>
                <b-col cols="12" class="circles-wrapper">
                  <div class='circle-wrapper' v-for="x in 3" :key="x">
                    <div class="circle">
                      </div>
                    </div>
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="12" class="expression-row" v-html="formattedExpression">
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="12" class="result-row">
                  {{ formattedResult }}
                </b-col>
              </b-row>
            </b-container>
          </div>
          <div class="controls">
            <b-container>
              <b-row>
                <b-col cols="3" class="operator" @click="onButtonClick(CLEAR)">
                  C
                </b-col>
                <b-col cols="3" class="operator" @click="onButtonClick(PERC)">
                  %
                </b-col>
                <b-col cols="3" class="operator" @click="onButtonClick(SQUARE_ROOT)">
                  &radic;
                </b-col>
                <b-col cols="3" class="operator" @click="onButtonClick(DIVIDE)">
                  &divide;
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="3" class="number" @click="onButtonClick(7)">
                  7
                </b-col>
                <b-col cols="3" class="number" @click="onButtonClick(8)">
                  8
                </b-col>
                <b-col cols="3" class="number" @click="onButtonClick(9)">
                  9
                </b-col>
                <b-col cols="3" class="operator" @click="onButtonClick(MULTIPLY)">
                  &times;
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="3" class="number" @click="onButtonClick(4)">
                  4
                </b-col>
                <b-col cols="3" class="number" @click="onButtonClick(5)">
                  5
                </b-col>
                <b-col cols="3" class="number" @click="onButtonClick(6)">
                  6
                </b-col>
                <b-col cols="3" class="operator" @click="onButtonClick(MINUS)">
                  -
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="3" class="number" @click="onButtonClick(1)">
                  1
                </b-col>
                <b-col cols="3" class="number" @click="onButtonClick(2)">
                  2
                </b-col>
                <b-col cols="3" class="number" @click="onButtonClick(3)">
                  3
                </b-col>
                <b-col cols="3" class="operator" @click="onButtonClick(ADD)">
                  +
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="3" class="number" @click="onButtonClick(0)">
                  0
                </b-col>
                <b-col cols="3" class="number" @click="onButtonClick(DOUBLE_ZERO)">
                  00
                </b-col>
                <b-col cols="3" class="operator" @click="onButtonClick(DECIMAL)">
                  .
                </b-col>
                <b-col cols="3" class="equals" @click="onEqualsClick()">
                  <div class="equals-wrapper">
                    =
                  </div>
                </b-col>
              </b-row>
            </b-container>
          </div>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import constants from './../constants.js'

function isNumber(item) {
  return Number.isInteger(item)
}
function getLastChar(string) {
  return string[string.length - 1]
}
export default {
  name: 'HelloWorld',
  data() {
    return {
      ...constants,
      expression: '',
      result: '0'
    }
  },
  computed: {
    formattedResult() {
      if (!this.result) {
        return 0
      }
      return this.result
    },
    formattedExpression() {
      if (this.expression === '') {
        return 0
      }
      let { expression } = this
      let result = expression
      result = result.replaceAll('+', ' + ')
      result = result.replaceAll('-', ' - ')
      result = result.replaceAll('*', ' &times; ')
      result = result.replaceAll('/', ' &divide; ')
      // result = result.replaceAll('', '&radic;')
      return result
    }
  },
  methods: {
    onEqualsClick() {
      try {
        let result = eval(this.expression)
        this.result = result
      } catch(e) {
        this.result = 'Invalid'
      }
    },
    onButtonClick(item) {
      if (item === 0) {
        const lastChar = parseInt(getLastChar(this.expression))
        if (isNumber(lastChar)) {
          this.expression += '0'
        }
      } else if (isNumber(item)) {
        this.expression += item
      } else {
        switch(item) {
          case constants.ADD: {
            this.expression += '+'
          }
          break
          case constants.MINUS: {
            this.expression += '-'
          }
          break
          case constants.MULTIPLY: {
            this.expression += '*'
          }
          break
          case constants.DIVIDE: {
            this.expression += '/'
          }
          break
          case constants.DECIMAL: {
            this.expression += '.'
          }
          break
          case constants.CLEAR: {
            this.expression = this.expression.slice(0, this.expression.length - 1)
          }
          break
          case constants.DOUBLE_ZERO: {
            const lastChar = parseInt(getLastChar(this.expression))
            if (isNumber(lastChar)) {
              this.expression += '00'
            }
          }
        }
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@import './../scss/variables.scss';
@import url('https://fonts.googleapis.com/css2?family=Quicksand&display=swap');


.calculator {
  width: 280px;
  .display {
    background: rgba(255, 255, 255, 0.45);
    backdrop-filter: blur(40px);
    /* Note: backdrop-filter has minimal browser support */
    border-top-left-radius: $calc-border-radius;
    border-top-right-radius: $calc-border-radius;
  }
  .controls {
    padding: 15px 0 15px 0;
    background: white;
    border-bottom-left-radius: $calc-border-radius;
    border-bottom-right-radius: $calc-border-radius;
    .col-3 {
      display: flex;
      justify-content: center;
      align-items: center;
      height: 40px;
    }
  }
}
.number {
  color: $magenta;
  cursor: pointer;
}
.operator {
  color: $pink;
  cursor: pointer;
}
.equals-wrapper {
  cursor: pointer;
  background: $pink;
  color: white;
  border-radius: 999px;
  flex: 1;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.calculator-wrapper {
  display: flex;
  align-items: center;
  justify-content: center;
}
.circles-wrapper {
  display: flex;
  padding-top: 15px;
  .circle-wrapper {
    width: 10px;
    height: 10px;
    margin-right: 3px;
    .circle {
      background: $pink-faded;
      border-radius: 999px;
      width: 10px;
      height: 10px;
    }
  }  
}
.expression-row, .result-row {
  text-align: right;
  color: $magenta;
}
.expression-row {
  padding-top: 15px;
  min-height: 24px;
}
.result-row {
  font-size: 2em;
  padding-bottom: 15px;
  word-break: break-all;
  min-height: 48px;
}
.main-container {
  display: flex;
  .main-row {
    flex: 1;
  }
}
</style>
