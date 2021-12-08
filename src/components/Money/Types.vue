
<template>
  <div>
    <ul class="types">
      <li :class="{selected:value==='-',[classPrefix+'-item']:classPrefix}"
      @click="selectType('-')">支出</li>
      <li :class="{selected:value==='+',[classPrefix+'-item']:classPrefix}"
      @click="selectType('+')">收入</li>
    </ul>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import {Component, Prop, Watch} from "vue-property-decorator";

@Component
export default class types extends Vue {
  @Prop(String) readonly value!:string;
  @Prop(String) classPrefix?:string
  //'-'表示指出，‘+’表示收入
  selectType(type:string) {
    if (type !== '-' && type !== '+') {
      throw new Error('type is unknown');
    }
    this.$emit('update:value',type)
  }

}


</script>

<style lang="scss" scoped>
.types{
  background: purple;
  display: flex;
  text-align: center;
  font-size: 24px;
  > li{
    color: #D9D9D9;
    width: 50%;
    line-height: 64px;
    height: 64px;
    justify-content: center;
    align-items: center;
    position: relative;
    &.selected::after{
      content: '';
      position: absolute;
      bottom:0;
      left: 0;
      width: 100%;
      height: 4px;
      background:yellowgreen;
    }
  }
}
</style>