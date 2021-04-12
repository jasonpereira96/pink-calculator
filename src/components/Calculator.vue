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
    <div class='bubble' v-for="(bubble, index) in bubbles" :style="bubble.style" :key="index">
      <img src="./../assets/bubble.png" />
    </div>
  </b-container>
</template>

<script>
import constants from './../constants.js'
import _ from 'lodash'

function isNumber(item) {
  return Number.isInteger(item)
}
function getRandomArbitrary(min, max) {
  return Math.random() * (max - min) + min;
}
function getStyle({ left, moveCloudsPeriod, sidewaysPeriod, scale }) {
  if (!scale) {
    scale = 1
  }
  return `animation: moveclouds ${moveCloudsPeriod}s linear infinite, sideways ${sidewaysPeriod}s ease-in-out infinite alternate; 
    left: ${left}px; transform: scale(${scale})`
}
export default {
  name: 'Calculator',
  data() {
    return {
      ...constants,
      expression: [],
      result: '0',
      bubbles: []
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
      if (this.expression.length === 0) {
        return 0
      }
      return this.expression.reduce((acc, token) => {
        if (token.isOperator) {
          return acc + ' ' + (token.displayChar || token.operator)
        } else {
          return acc + ' ' + token.value
        }
      }, '')
    }
  },
  mounted() {
    
    let bubblesConfig = []
    for (let count = 0; count < 300; count++) {
      bubblesConfig.push({
        // left: 10 + count * 10,
        left: getRandomArbitrary(10, 1300),
        moveCloudsPeriod: getRandomArbitrary(3, 30),
        scale: getRandomArbitrary(0.1, 2),
        sidewaysPeriod: getRandomArbitrary(1, 10)
      })
    }
    this.bubbles = bubblesConfig.map(config => {
      return {
        style: getStyle(config)
      }
    })
    window.addEventListener('keyup', this.onKeyUp);
  },
  methods: {
    onEqualsClick() {
      let expression = _.cloneDeep(this.expression)
      let expressionString = ''
      try {
        for (let index=0;index<expression.length;index++) {
          let token = expression[index]
          if (token.isOperator) {
            if (token.operator === '%') {
              expressionString += '/100'
            } else if (token.operator === 'ROOT') {
              let nextToken = expression[index + 1]
              if (nextToken && !nextToken.isOperator) {
                nextToken.value = nextToken.value ** 0.5
              } else {
                throw new Error()
              }
              let prevToken = expression[index - 1]
              if (prevToken && !prevToken.isOperator) {
                expressionString += '*'
              }
            } else {
              expressionString += token.operator
            }
          } else {
            expressionString += token.value
          }
        }
        let result = eval(expressionString)
        this.result = result
      } catch(e) {
        this.result = 'Invalid'
      }
    },
    getLastToken() {
      return this.expression[this.expression.length - 1]
    },
    onButtonClick(item) {
      if (this.expression.length === 0) {
        if (isNumber(item)) {
          this.expression.push({
            value: `${item}`,
            isOperator: false
          })  
        } else if (item === constants.SQUARE_ROOT) {
          this.expression.push({
            isOperator: true,
            operator: 'ROOT',
            displayChar: '&radic;'
          })
        } else if (item === constants.MINUS) {
          this.expression.push({
            isOperator: true,
            operator: '-'
          })
        }
        return
      }
      if (item === 0) {
        let lastToken = this.getLastToken()
        if (!lastToken.isOperator) {
          lastToken.value += '0'
        }
      } else if (isNumber(item)) {
        let lastToken = this.getLastToken()
        if (lastToken.isOperator) {
          this.expression.push({
            value: `${item}`,
            isOperator: false
          })
        } else {
          lastToken.value += item
        }
      } else {
        switch(item) {
          case constants.ADD: {
            this.expression.push({
              isOperator: true,
              operator: '+'
            })
          }
          break
          case constants.MINUS: {
            this.expression.push({
              isOperator: true,
              operator: '-'
            })
          }
          break
          case constants.MULTIPLY: {
            this.expression.push({
              isOperator: true,
              displayChar: '&times;',
              operator: '*'
            })
          }
          break
          case constants.DIVIDE: {
            this.expression.push({
              isOperator: true,
              displayChar: '&divide;',
              operator: '/'
            })
          }
          break
          case constants.DECIMAL: {
            let lastToken = this.getLastToken()
            if (!lastToken.isOperator) {
              lastToken.value += '.'
            }
          }
          break
          case constants.CLEAR: {
            this.expression.pop()
          }
          break
          case constants.DOUBLE_ZERO: {
            let lastToken = this.getLastToken()
            if (!lastToken.isOperator) {
              lastToken.value += '00'
            }
          }
          break
          case constants.PERC: {
            this.expression.push({
              isOperator: true,
              operator: '%'
            })
          }
          break
          case constants.SQUARE_ROOT: {
            this.expression.push({
              isOperator: true,
              operator: 'ROOT',
              displayChar: '&radic;'
            })
          }
        }
      }
    },
    onKeyUp(event) {
      console.log(event)
      if (Number.isInteger(parseInt(event.key))) {
        this.onButtonClick(parseInt(event.key))
      } else {
        if (event.key === "+") {
          this.onButtonClick(constants.ADD)
        } else if (event.key === '-') {
          this.onButtonClick(constants.MINUS)
        } else if (event.key === "*") {
          this.onButtonClick(constants.MULTIPLY)
        } else if (event.key === '/') {
          this.onButtonClick(constants.DIVIDE)
        } else if (event.key === 'Enter') {
          this.onEqualsClick()
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
  z-index: 5;
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
.bubble {
  width: 40px;
  height: 40px;
  position: absolute;
  display: flex;
  img {
    flex: 1;
  }
  // animation: moveclouds 15s linear infinite, sideways 1s ease-in-out infinite alternate;
}

</style>
<style lang="scss">
@keyframes moveclouds { 
  0% { 
    top: 500px;
  }
  100% { 
    top: -500px;
  }
}

@keyframes sideways { 
  0% { 
    margin-left: 0px;
  }
  100% { 
    margin-left: 50px;
  }
}
</style>
