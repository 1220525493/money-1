
<template>
  <div class="numberPad">
    <div class="output">{{output || '&nbsp;'}}</div>
    <div class="buttons" >
      <button @click="inputContent">1</button>
      <button @click="inputContent">2</button>
      <button @click="inputContent">3</button>
      <button @click="remove">删除</button>
      <button @click="inputContent">4</button>
      <button @click="inputContent">5</button>
      <button @click="inputContent">6</button>
      <button @click="clear">清空</button>
      <button @click="inputContent">7</button>
      <button @click="inputContent">8</button>
      <button @click="inputContent">9</button>
      <button @click="ok" class="ok">OK</button>
      <button @click="inputContent" class="zero">0</button>
      <button @click="inputContent">.</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import {Component, Prop,} from "vue-property-decorator";
import Button from "@/components/Button.vue";
@Component({
  components: {Button}
})
export default class NumberPad extends Vue {
  @Prop(Number) readonly value!:number;
  output = this.value.toString();
  inputContent(event: MouseEvent) {
    const button = (event.target as HTMLButtonElement);
    const input = button.textContent as string;
    if (this.output.length === 16){return;}
    if (this.output === '0') {
      if (input === '0') {return}
      if ('0123456789'.indexOf(input) >= 0) {
        this.output = input;
      } else {
        this.output += input;
      }
      return;
    }
    if (this.output.indexOf(".") >= 0 && input === ".") {return;}
    this.output += input;
  }
  remove(){
    if (this.output.length===1){
      this.output='0';
    }else {
      this.output = this.output.slice(0,-1);
    }
  }
  clear(){
    this.output='0';
  }
  ok(){
    const number = parseFloat(this.output)
 this.$emit('update:value',number)
    this.$emit('submit',number)
    this.output='0';
  }

}
</script>

<style lang="scss" scoped>
.numberPad {
  @import "src/assets/style/helper.scss";
    color: lightpink;
  .output {
    @extend %clearFix;
    @extend %innerShadow;
    font-size: 36px;
    font-family: Consolas, monospace;
    padding: 9px 16px;
    text-align: right;
    box-shadow: inset 0 -5px 5px -5px fade_out(black, 0.5),
    inset 0 -5px 5px -5px fade_out(black, 0.5);
  }

  .buttons {
    @extend %clearFix;
    flex-wrap: wrap;
     >button:active{
      box-shadow: 0 0 4px 0 rgba(0,0,0,.15);
       color: rgba(12,164,190);
       text-shadow: 0 0 15px #57c1f1;
    }
    > button {
      width: 25%;
      height: 64px;
      float: left;
      background: transparent;
      border: none;
      box-shadow: 0 1px 10px rgba(255,255,255), 0 1px rgba(0,0,0,0.3);
      border-radius: 15px;
      text-align: center;
      line-height: 5em;
      transition: all 0.25s;
      color: #eb3f33;
      &.ok {
        height: 64*2px;
        float: right;
      }
      &.zero {
        width: 25*2%;

      }

    }
  }
}
</style>