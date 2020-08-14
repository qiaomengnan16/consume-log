<template>
  <Layout>
    <div class="tags">
      <router-link class="tag"
                   v-for="tag in tags" :key="tag.id"
                   :to="`/labels/edit/${tag.id}`">
        <span>{{tag.name}}</span>
        <Icon name="right"/>
      </router-link>
    </div>
    <div class="createTag-wrapper">
      <Button @click.native="createTag">新建标签</Button>
    </div>
  </Layout>
</template>

<script lang="ts">
  import {Component} from "vue-property-decorator";
  import Button from "@/components/Button.vue";
  import tagHelper from "@/mixins/TagHelper";
  import {mixins} from "vue-class-component";

  @Component({
    components: {Button}
  })
  export default class Labels extends mixins(tagHelper) {
    get tags() {
      return this.$store.state.tagList
    }

    created() {
      this.$store.commit('fetchTags')
    }
  }
</script>

<style scoped lang="scss">
  .tags {
    background-color: white;
    font-size: 16px;
    padding-left: 16px;

    > .tag {
      min-height: 44px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid #e6e6e6;

      svg {
        color: #666;
        margin-right: 16px;
        height: 18px;
        width: 18px;
      }
    }
  }

  .createTag-wrapper {
    text-align: center;
    padding: 16px;
    margin-top: 44-16px;
  }

</style>