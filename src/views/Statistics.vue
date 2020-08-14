<template>
  <Layout>
    <Tabs class-prefix="types" :data-source="recordTypeList" :value.sync="type"></Tabs>
    <!--    <Tabs class-prefix="interval" :data-source="intervalList" :value.sync="interval"></Tabs>-->
    <ol>
      <li v-for="(group,index) in groupList" :key="index">
        <h3 class="title">{{beautify(group.title)}} <span>{{group.total}}</span></h3>
        <ol>
          <li class="record" v-for="item in group.items" :key="item.id">
            <span>{{tagString(item.tags)}}</span>
            <span class="notes">{{item.notes}}</span>
            <span>￥{{item.amount}}</span></li>
        </ol>
      </li>
    </ol>
  </Layout>
</template>

<script lang="ts">
  import Vue from "vue"
  import Types from "@/components/Money/Types.vue"
  import {Component} from "vue-property-decorator";
  import Tabs from "@/components/Tabs.vue";
  import intervalList from "@/constants/intervakList";
  import recordTypeList from "@/constants/recordTypeList";
  import dayjs from "dayjs"
  import clone from "@/lib/clone";

  @Component({
    components: {Types, Tabs}
  })
  export default class Statistics extends Vue {
    get recordList() {
      return (this.$store.state as RootState).recordList
    }

    get groupList() {
      const {recordList} = this
      const newList = clone(recordList).filter(r => r.type === this.type).sort((a, b) => dayjs(b.createdAt).valueOf() - dayjs(a.createdAt).valueOf())
      type Result = { title: string; items: RecordItem[]; total?: number }[]
      const result: Result = [{
        title: dayjs(newList[0].createdAt).format("YYYY-MM-DD"),
        items: [newList[0]]
      }]

      for (let i = 0; i < newList.length; i++) {
        const current = newList[i];
        const last = result[result.length - 1]
        if (dayjs(last?.title).isSame(dayjs(current.createdAt), 'day')) {
          last?.items.push(current)
        } else {
          result.push({title: dayjs(current.createdAt).format("YYYY-MM-DD"), items: [current]})
        }
      }
      result.map(group => {
        group.total = group.items.reduce((sum, item) => sum + item.amount, 0)
      })

      return result
    }

    tagString(tags: Tag[]) {
      return tags.length === 0 ? '无' : tags.join(',')
    }

    beautify(string: string) {
      const day = dayjs(string)
      const now = dayjs()
      if (day.isSame(now, 'day')) {
        return "今天"
      } else if (day.isSame(now.subtract(1, 'day'), 'day')) {
        return "昨天"
      } else if (day.isSame(now.subtract(2, 'day'), 'day')) {
        return "前天"
      } else if (day.isSame(now, 'year')) {
        return day.format("MM月D日")
      } else {
        return day.format("YYYY年MM月D日")
      }
    }

    beforeCreate() {
      this.$store.commit('fetchRecords')
    }

    type = '-'
    interval = 'day'
    intervalList = intervalList
    recordTypeList = recordTypeList
  }
</script>

<style scoped lang="scss">
  %item {
    padding: 0 16px;
    min-height: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .title {
    padding: 0 16px;
  }

  .record {
    background-color: white;
    @extend %item;
  }

  .notes {
    margin-right: auto;
    margin-left: 8px;
    color: #999;
  }

  ::v-deep {
    .types-tabs-item {
      background-color: #c4c4c4;

      &.selected {
        background-color: white;

        &::after {
          display: none;
        }
      }
    }

    .interval-tabs-item {
      height: 48px;
    }
  }
</style>