<template>
  <div ref="v_vigna" class="full">
    <!-- Loading Cesium-Vigna. -->
    <dev-pannel :instance="instance" class="dev-pannel" />
  </div>
</template>
<script lang="ts">
/*eslint-disable no-debugger*/

import { Component, Vue } from 'vue-property-decorator';
// const Vigna = require('@msign/vigna').default;
import Vigna from '@msign/vigna/src';

@Component({
  components: {
    DevPannel: () => import('./UIDev/Pannel.vue')
  }
})
export default class ComCesium extends Vue {
  private instance?: any = null;
  private async _loadVigna() {
    const {
      $refs: { v_vigna }
    } = this;

    const vigna = new Vigna({
      container: v_vigna as HTMLElement
    });
    const { instance } = vigna;
    this.instance = instance as any;
    // const cSc = await Instance.getInstanceSceneControl();
    this.$emit('load', instance);
  }

  private async _initall() {
    await this._loadVigna();
  }
  async mounted() {
    await this._initall();
  }
}
</script>
<style scoped>
.full {
  height: 100%;
  width: 100%;
}
.dev-pannel {
  z-index: 9;
  /* pointer-events: none; */
  position: absolute;
}
</style>
