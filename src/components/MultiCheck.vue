<template>
  <div class="box-wrapper">
    <div>@todo</div>
    <input type="checkbox" v-model="allChecked" value="all" @change="changeAll">全选
    <div v-for="(item, idx) in opts" :key="idx" class="options">
      <input type="checkbox" v-model="arr" :value="item.value" @change="changeOne">{{item.label}}
    </div>
  </div>
    
</template>

<script lang="ts">
import { defineComponent, toRefs, ref } from 'vue';

export type Option = {
	label: string;
	value: string;
};

/**
 * Notice:
 * 1. There should be a special `Select All` option with checkbox to control all passing options
 * 2. If columns > 1, the options should be placed from top to bottom in each column
 *
 * @param {string} label - the label text of this component
 * @param {Option[]} options - options
 * @param {string[]} values - default checked option values
 * @param {number} columns - default value is 1
 * @param {Function} onChange - when checked options are changed,
 *                             they should be passed to outside
 */

export default defineComponent({
  name: 'MultiCheck',
  props: ['value', 'opts'],
  emits:['updateValue'],
  setup(props, context) {
    
    const data = toRefs(props)
    let isChecked = ref(false)
    let allChecked = ref(false)
    
    let arr = ref([])
    function changeOne() {
      allChecked.value = arr.value.length === data.opts.value.length
      context.emit('updateValue', arr.value)
    }
    function changeAll() {
      if (allChecked.value) {
        arr.value = props.opts.map((item: Option) => item.value)
      } else {
        arr.value = []
      }
      context.emit('updateValue', arr.value)
    }
    return {
      arr,
      isChecked,
      allChecked,
      ...data,
      changeOne,
      changeAll
    }
  }
  
});
</script>

<style scoped lang="less">
  .box-wrapper {
    width: 300px;
    text-align: left;
    margin: 0 auto;
  }
  .options {
    text-indent: 20px;
  }
</style>
