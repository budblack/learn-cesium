<template>
  <div id="app">
    <div class="bg">
      <span>DEV APP</span>
    </div>
    <ComDev class="fg" />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import ComDev from './components/ComDev/index.vue';
import 'normalize.css';

@Component({
  components: {
    ComDev
  }
})
export default class App extends Vue {}
</script>

<style scope lang="scss">
html,
body,
* {
  margin: 0;
  padding: 0;
}
html {
  width: 100%;
  height: 100%;
}
body {
  margin: 0;
  padding: 0;
  position: absolute;
  width: 100%;
  height: 100%;
  background: aliceblue;
  display: flex;
  justify-content: center;
  align-items: center;
}
#app {
  background: yellowgreen;
  width: calc(100% - 36px);
  height: calc(100% - 36px);
  position: relative;
}
.fg {
  position: absolute;
  // opacity: 0.9;
}
.bg {
  position: absolute;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: fantasy;
}
</style>
