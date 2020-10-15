#### 概述   

```markdown
 aui-tree 基于 element tree + iview 二次封装的架构组件 
```

------


#### 代码示例

```vue
<template>
    <AuiSplitLayout :layout="layout" splitValue="275px" >
        <template slot="tree">
             <AuiTree :createTree="createTree"
                ref="auiTreeRef"
                @on-tree-callback="treeLoadedCallback"
                @on-node-click="handleTreeNodeClick"
                @on-opts-click="handleTreeOptsClick"
                @on-custom-icon-click="handleCustomIconClick"/>
        </template>
    </AuiSplitLayout>
</template>
<script>
// 当前对象。不需要动态观察监听
let currentTreeObj = {
    treeNode: {},
    optTreeNode：{},
    label: ''
};
export default {
    data() {
        return {
			createTree: {
                url: `${app.root}/department/GetClassTreeForAdmin`,
                dataFilter: (item) => {
                    if (item.AllowAddChild && item.AllowEdit) {
                        item.opts = ['添加', '编辑', '删除'];
                    } else if (item.AllowAddChild) {
                        item.opts = ['添加'];
                    } else if (item.AllowEdit) {
                        item.opts = ['编辑', '删除'];
                    }
                }
            }
        };
    },
    methods: {
		// 架构加载完成
    	treeLoadedCallback (node) {
			// do something: 获取表格
		},
        // 点击架构获取下一级
        handleTreeNodeClick (node) {
            // do something: 更新表格，获取按钮组
        },
        // 架构操作栏点击回调
        handleTreeOptsClick (label, node, data) {
            // 全局对象赋值。固定写法
            currentTreeObj.treeNode = node;
            currentTreeObj.optTreeNode = JSON.parse(JSON.stringify(data));
            currentTreeObj.label = label;
            
            switch (label) {
                case '添加':
                    // do something
                    break;
                case '编辑':
                    // do something
                    break;
                case '删除':
                    // do something
                    break;
            }
        },
        // 按钮点击回掉
        handleCustomIconClick () {}
    }
};
</script>
```

------

#### API  

###### Props

| 属性         | 说明               | 类型    | 默认值 |
| ------------ | ------------------ | ------- | ------ |
| sortHide     | 隐藏排序icon       | Boolean | false  |
| uploadHide   | 隐藏导入icon       | Boolean | false  |
| treeOptsHide | 隐藏架构架构操作栏 | Boolean | false  |
| createTree   | 架构创建对象       | Object  | {}     |

###### Props：createTree

| 属性         | 说明                                                         | 类型     | 默认值 |
| ------------ | ------------------------------------------------------------ | -------- | ------ |
| url          | 获取架构接口                                                 | String   | ''     |
| staticParams | 获取架构额外参数                                             | Object   | {}     |
| opts         | 架构操作栏配置                                               | Boolean  | ''     |
| dataFilter   | 预处理组织架构数据，常用于前端控制操作栏展示项。直接操作源数据，无返回值 | Function | -      |

###### Events

| 事件名               | 说明                                           | 返回值            |
| -------------------- | ---------------------------------------------- | ----------------- |
| on-tree-callback     | 架构渲染完成回调                               | 根节点数据        |
| on-node-click        | 选人组件输入框删除已选内容后触发               | node, data, event |
| on-opts-click        | 架构操作栏点击回调                             | name, node, data  |
| on-custom-icon-click | 自定义按钮回调，只触发。这里代码待优化，太死了 | 无                |

###### Methods

| 事件名       | 说明                     | 参数             | 返回值 |
| ------------ | ------------------------ | ---------------- | ------ |
| append       | 在父级节点最后追加新节点 | node, parentNode | 无     |
| insertBefore | 在操作节点前加一个新节点 | newNode, node    | 无     |
| insertAfter  | 在操作节点后加一个新节点 | newNode, node    | 无     |
| remove       | 删除节点                 | node             | 无     |