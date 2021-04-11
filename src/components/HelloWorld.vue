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
                <b-col cols="12" class="expression-row">
                  34 + 35 + 400 + 20
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="12" class="result-row">
                  489
                </b-col>
              </b-row>
            </b-container>
          </div>
          <div class="controls">
            <b-container>
              <b-row>
                <b-col cols="3" class="operator">
                  C
                </b-col>
                <b-col cols="3" class="operator">
                  %
                </b-col>
                <b-col cols="3" class="operator">
                  &radic;
                </b-col>
                <b-col cols="3" class="operator">
                  &divide;
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="3" class="number">
                  7
                </b-col>
                <b-col cols="3" class="number">
                  8
                </b-col>
                <b-col cols="3" class="number">
                  9
                </b-col>
                <b-col cols="3" class="operator">
                  &times;
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="3" class="number">
                  4
                </b-col>
                <b-col cols="3" class="number">
                  5
                </b-col>
                <b-col cols="3" class="number">
                  6
                </b-col>
                <b-col cols="3" class="operator">
                  -
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="3" class="number">
                  1
                </b-col>
                <b-col cols="3" class="number">
                  2
                </b-col>
                <b-col cols="3" class="number">
                  3
                </b-col>
                <b-col cols="3" class="operator">
                  +
                </b-col>
              </b-row>
              <b-row>
                <b-col cols="3" class="number">
                  0
                </b-col>
                <b-col cols="3" class="number">
                  00
                </b-col>
                <b-col cols="3" class="operator">
                  .
                </b-col>
                <b-col cols="3" class="equals">
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
export default {
  name: 'HelloWorld',
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
}
.result-row {
  font-size: 2em;
  padding-bottom: 15px;
}
.main-container {
  display: flex;
  .main-row {
    flex: 1;
  }
}
</style>
