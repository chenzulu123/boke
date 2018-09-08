## Vue的生命周期

### 1、beforeCreate
组件实例刚被创建，组件属性计算之前，如data属性等
### 2、created
组件实例创建完成，属性已绑定，但DOM还未生成，$el属性还不存在
### 3、beforeMount
模板编译/挂载之前
### 4、mounted
模板编译/挂载之后
### 5、beforeUpdate
组件更新之前
### 6、updated
组件更新之后
### 7、beforeDestory
组件销毁前调用
### 8、destoryed
组件销毁后调用