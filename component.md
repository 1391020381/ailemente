# form 表单组件
1. Form.vue   model  rules provide(model,rules) 监听addFormItem 事件 保存被添加FormItem的validate
2. FormItem.vue    onMounted 监听 validate 并触发addFormItem
3. Input.vue 实现双绑定