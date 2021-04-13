<template>
  <div id="app">
    <Calculator />
    <b-container fluid class="footer">
      <div>Code: Jason Pereira
        <span>
          <a href="https://github.com/jasonpereira96">
            <b-icon icon="github"></b-icon>
          </a>
        </span>
      </div>
      <div>Design: Annabelle D'Souza 
        <span>
          <a href="https://www.instagram.com/hidden.kitten.designs">
            <b-icon icon="instagram"></b-icon>
          </a>
        </span>
      </div>
    </b-container>
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue'

export default {
  name: 'App',
  components: {
    Calculator
  }
}
</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Quicksand&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@700&display=swap');

@import 'scss/variables.scss';
html, body, #app {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
#app {
  font-family: Quicksand;
  font-size: 30px;
  font-style: normal;
  font-weight: 400;
  // line-height: 94px;
  letter-spacing: 0em;
  text-align: left;
  background: $light-pink;
  display: flex;
  flex-direction: column;
}
.footer {
  font-family: Inter;
  font-style: normal;
  font-size: 16px;
  font-weight: 700;
  letter-spacing: 0em;
  text-align: left;
  padding-top: 15px;
  padding-bottom: 15px;
  color: $magenta;
  text-transform: uppercase;
  font-weight: bold;
  div {
    text-align: center;
    padding: 5px 0;
    a {
      color: $magenta;
    }
    svg {
      cursor: pointer;
      transform: scale(1.5);
      margin-left: 10px;
    }
  }
}
</style>
