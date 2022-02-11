<template>
  <div class="el-form">
    <slot />
  </div>
</template>
<script lang="ts">
export default{
  name:'ElForm'
}

</script>

<script setup lang="ts">
import { PropType, provide } from "vue"
import { Rules } from "async-validator"
import { ref } from "vue"
import { emitter } from "../../emitter"
import { FormItem, key } from "./type"

const props = defineProps({
  model: { type: Object, required: true },
  rules: { type: Object as PropType<Rules> },
})

provide(key, {
  model: props.model,
  rules: props.rules,
})

const items = ref<FormItem[]>([])

emitter.on("addFormItem", (item) => {
  items.value.push(item)
})

function validate(cb: (isValid: boolean) => void) {
  const tasks = items.value.map((item) => item.validate())
  Promise.all(tasks)
    .then(() => { cb(true) })
    .catch(() => { cb(false) })
}
// 使用 <script setup>的组件默认关闭的,也即通过模板ref或者$parent链获取到组件的公开实例,不会暴露任何在 <script setup>中声明的绑定
//  为了在 <script setup>组件中明确要暴露出去的属性,使用defineExpose编译器宏
defineExpose({
  validate,
})
</script>

<style lang="scss">
@import '../styles/mixin';
@include b(form) {
  margin-top:20px;
  box-sizing: border-box;
  flex-shrink: 0;
  width:300px;
}

</style>