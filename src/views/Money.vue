<template>
  <div>
    <Layout class-prefix="layout">
     <NumberPad :value.sync="record.amount" @submit="saveRecord" />
      <Tabs :data-source="typeList" :value.sync="record.type"/>
      <div class="notes">
        <FromItem field-name="备注"
                  placeholder="在这里输入备注"
                  @update:value="onUpdateNotes"/>
      </div>
     <Tags/>
    </Layout>
  </div>

</template>

<script  lang="ts">
import NumberPad from "@/components/Money/NumberPad.vue";
import FromItem from "@/components/Money/FromItem.vue";
import Tags from "@/components/Money/Tags.vue";
import {Component, Vue,} from "vue-property-decorator";
import typeList from "@/constants/typeList";
import Tabs from "@/components/Tabs.vue";





@Component({
  components: {Tabs, Tags,  FromItem,  NumberPad},
})
export default class Money extends Vue{
  get recordList(){
    return this.$store.state.recordList;
  }

  record:RecordItem={tags:[],notes:'',type:'-',amount:0};
  created(){
    this.$store.commit('fetchRecords')
  }

  typeList = typeList;

  onUpdateNotes(value:string){
   this.record.notes=value;
  }
  saveRecord(){
 this.$store.commit('createRecord',this.record)
  }

}
</script>
<style lang="scss">
.layout-content{
display: flex;
  flex-direction: column-reverse;
}
.notes{
  padding: 12px 0;
}
</style>
