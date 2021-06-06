# vue-thousandth-input

vue-thousandth-input component for Vue.js / 基于vue的一个千分位输入表单组件


## Props / 参数

| Property | Type | Default value | Description |
| -------- | ---- | ------------- | ----------- |
| `value` | `String`|`''`  | Default value / 默认值 |
| `v-model` | `String`|`value`  | v-model Default value / v-model默认值 |
| `placeholder` | `String` | `''` | placeholder for the input |

### event / 事件

| Event | Arguments | Description | Notes |
| ----- | --------- | ----------- | ----- |
| `change` | `Object` | Fires when the input changes with the argument is the object includes `{ event, changeValue, value }`  / 输入事件返回的值 | onInput |
| `focus` | `Object` | Fires when the focus with the argument is the object includes `{ event, changeValue, value }`  / 获取焦点事件返回的值 | onFocus Available from v0.1.2 / v0.1.2版本支持 |
| `blur` | `Object` | Fires when the blur with the argument is the object includes `{ event, changeValue, value }`  / 失去焦点返回的值 | onBlur Available from v0.1.2 / v0.1.2版本支持 |
"# vue-thousandth-input" 
