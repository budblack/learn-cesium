<template>
  <div class="dev-container">
    <component :is="current_com" />
  </div>
</template>
<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import VCesium from './VCesiumVigna/index.vue';

@Component({
  data() {
    return {
      current_com: null
    };
  }
})
export default class ComCesium extends Vue {
  current_com?: typeof Vue;
  mounted() {
    this.current_com = VCesium;
  }
}
</script>
<style scoped lang="scss">
$MARGIN: 0px;

.dev-container {
  height: calc(100% - 2 * #{$MARGIN});
  width: calc(100% - 2 * #{$MARGIN});
  margin: $MARGIN;
  background: #f1f1f1;
  box-shadow: 0 0 0 1px #ff0000;
}
</style>
