<template>
  <div>
    <el-button size="mini">回到默认位置</el-button>

    <el-button size="mini" @click="toggle_terr">地形开关</el-button>
    <el-button size="mini" @click="toggle_boundingbox">包围盒开关</el-button>

    <el-button size="mini" @click="load_3dtile('oblique')">
      载入倾斜摄影
    </el-button>
    <el-button size="mini" @click="load_3dtile('model')">
      载入实体模型
    </el-button>
    <el-button size="mini" @click="flyto_l3dtile">聚焦倾斜摄影</el-button>

    <el-input v-model="ly_mv_name" size="mini" style="width:8em;"></el-input>
    <el-input v-model="ly_mv_step" size="mini" style="width:2em;"></el-input>
    <el-button size="mini" @click="mvly([ly_mv_step, 0, 0])">X+</el-button>
    <el-button size="mini" @click="mvly([-ly_mv_step, 0, 0])">X-</el-button>
    <el-button size="mini" @click="mvly([0, ly_mv_step, 0])">Y+</el-button>
    <el-button size="mini" @click="mvly([0, -ly_mv_step, 0])">Y-</el-button>
    <el-button size="mini" @click="mvly([0, 0, ly_mv_step])">Z+</el-button>
    <el-button size="mini" @click="mvly([0, 0, -ly_mv_step])">Z-</el-button>
  </div>
</template>
<script lang="ts">
/*eslint-disable*/

import { Component, Vue, Prop } from 'vue-property-decorator';
import Instance from '@msign/vigna/src/core/Instance';

@Component({})
export default class UIDevPannel extends Vue {
  @Prop({ default: null }) instance!: Instance;

  private _ly_terrain: any;
  private _ly_model: any;

  private ly_mv_name = '_ly_model';
  private ly_mv_step: number = 10;

  private mvly([x, y, z]: number[]) {
    const ly = (this as any)[this.ly_mv_name];
    if (!ly) {
      console.log('无此图层'); 
      return;
    }
    ly.root.transform[12] += x * 1;
    ly.root.transform[13] += y * 1;
    ly.root.transform[14] += z * 1;
  }
  private async _load_3dtile_oblique() {
    // 载入倾斜摄影
    const { instance: inst } = this;
    const cSc = inst.getInstanceSceneControl();
    const URL_Tileset_Oblique = `https://gis.7cdn.msign.net/zt/5um9/testdata/3dtiles/[20200402095633]NZX_Road_No3_OBLIQUE/part_1/tileset.json`;
    const ly: any = await cSc.loadLayer3DTile(URL_Tileset_Oblique);
    this._ly_terrain = ly;

    ly.debugShowBoundingVolume = false;
    (window as any).layer_terr = ly;
    return ly;
  }
  private async _load_3dtile_model() {
    const { instance: inst } = this;
    const cSc = inst.getInstanceSceneControl();
    const URL_Tileset_Oblique = `https://dl.7cdn.msign.net/zt/5um9/dev/2020/04/44363/tileset_mv2nzx.json`;
    const ly: any = await cSc.loadLayer3DTile(URL_Tileset_Oblique);
    this._ly_model = ly;

    ly.debugShowBoundingVolume = false;
    (window as any).layer_mode = ly;
    return ly;
  }

  async load_3dtile(type: string) {
    switch (type) {
      case 'oblique':
        await this._load_3dtile_oblique();
        break;
      case 'model':
        await this._load_3dtile_model();
        break;
    }
  }

  flyto_l3dtile() {
    const { instance: inst } = this;
    const cSc = inst.getInstanceSceneControl();
    const { viewer } = cSc;
    viewer.flyTo(this._ly_terrain);
  }

  toggle_terr() {
    const { instance: inst } = this;
    const cSc = inst.getInstanceSceneControl();
    const { viewer } = cSc;
    viewer.scene.globe.show = !viewer.scene.globe.show;
  }

  toggle_boundingbox() {
    this._ly_terrain.debugShowBoundingVolume = !this._ly_terrain
      .debugShowBoundingVolume;
  }
  async mounted() {
    await this._load_3dtile_oblique();
    // await this._load_3dtile_model();

    // this.toggle_terr();
    // this.toggle_boundingbox();
  }
}
</script>
