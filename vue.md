# study-notes
1.在VUE中，如何给通过app.use(组件)，全局绑定组建？

`import KindEditor from './KindEditor'`

`const vueEditor = function (Vue) {`

 `if (vueEditor.installed) return`

 `vueEditor.installed = true`

 `Vue.component('editor', KindEditor)`

`}`

`export default vueEditor`