<template>
  <Layout class-prefix="layout">
    <NumberPad :value.sync="record.amount" @submit="saveRecord"/>
    <!--    <Types :value.sync="record.type"/>-->
    <Tabs :data-source="recordTypeList" :value.sync="record.type"></Tabs>
    <div class="form-wrapper">
      <FormItem field-name="备注"
                placeholder="请在这里输入备注"
                @update:value="onUpdateNotes"/>
    </div>
    <Tags @update:value="onUpdateTags"/>
  </Layout>
</template>

<script lang="ts">
  import Vue from 'vue';
  import Types from '@/components/Money/Types.vue';
  import FormItem from '@/components/Money/FormItem.vue';
  import NumberPad from '@/components/Money/NumberPad.vue';
  import Tags from '@/components/Money/Tags.vue';
  import {Component} from 'vue-property-decorator';
  import Tabs from "@/components/Tabs.vue";
  import recordTypeList from "@/constants/recordTypeList";


  // const version = window.localStorage.getItem('version') || '0';
  // const recordList: Record[] = JSON.parse(window.localStorage.getItem('recordList') || '[]');
  // if (version === '0.0.1') {
  //   recordList.forEach(record => {
  //     record.createdAt = new Date(2020, 0, 1);
  //   });
  //   window.localStorage.setItem('recordList', JSON.stringify(recordList));
  // }
  // window.localStorage.setItem('version', '0.0.1');


  @Component({
    components: {Tabs, FormItem, NumberPad, Tags},
  })
  export default class Money extends Vue {
    record: RecordItem = {tags: [], type: '-', notes: '', amount: 0};

    recordTypeList = recordTypeList

    get recordList() {
      return this.$store.state.recordList
    }

    created() {
      this.$store.commit('fetchRecords')
    }

    onUpdateNotes(value: string) {
      this.record.notes = value;
    }

    saveRecord() {
      this.$store.commit('createRecord', this.record)
    }

    onUpdateTags(value: Tag[]) {
      this.record.tags = value;
    }
  }
</script>

<style lang="scss" scoped>
  ::v-deep .layout-content {
    display: flex;
    flex-direction: column-reverse;
  }
</style>

<style scoped lang="scss">
  .form-wrapper {
    padding: 12px 0
  }

</style>