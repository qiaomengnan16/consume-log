<template>
  <div>
    <label class="formItem">
      <span class="name">{{fieldName}}</span>
      <input type="text"
             :value="value"
             @input="onValueChanged($event.target.value)"
             :placeholder="placeholder">
    </label>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue';
  import {Component, Prop, Watch} from 'vue-property-decorator';

  @Component
  export default class FormItem extends Vue {
    @Prop({default: ''}) value!: string;

    @Prop({required: true}) fieldName!: string
    @Prop() placeholder?: string

    onValueChanged(value: string) {
      this.$emit('update:value', value);
    }
  }
</script>

<style scoped lang="scss">
  .formItem {
    font-size: 14px;
    display: flex;
    padding-left: 16px;
    align-items: center;

    .name {
      padding-right: 16px;
    }

    input {
      height: 40px;
      flex-grow: 1;
      background-color: transparent;
      border: none;
      padding-right: 16px;
    }
  }

</style>