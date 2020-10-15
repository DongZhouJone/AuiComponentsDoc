

#### 概述   

```markdown
 aui-chosen 基于 element tree + iview 二次封装的弹窗选人组件 
 优点： 纯 vue 版本，使用简单。通过同一个接口的不同参数配置实现各个业务
 缺点： 组件只在每次进入页面时实例化一次, 如果需要重新加载，需要手动实例化。 项目中使用太分散, 理论上是可以统一配置的。
```
****

#### 选人接口参数

| 参数             | 必填 | 示例       | 备注                                                         |
| ---------------- | ---- | ---------- | ------------------------------------------------------------ |
| Id               | 否   | 0          | 当前节点id，用于获取下级架构，默认不传                       |
| CheckType        | 是   | 0          | 可选架构类型：<br/>0 // 架构和用户均可勾选<br/>1 // 只用户可勾选<br/>2 // 只架构可勾选<br/>3 //只班级可勾选 包括行政班和课程班， |
| DepartmentType   | 是   | 1, '1,2,3' | 展示架构类型，可以单独使用id, 可以字符串组合：<br/>0 // 学校<br/>1 // 学生(行政班)<br/>2 // 教职工<br/>4 // 校友<br/>5 // 退休教师<br/>6 // 临时组<br/>7 // 虚拟组<br/>8 // 课程班<br/>10 // 教学班<br/>特殊场景参数组合：<br>只显示行政班：1’<br/>只显示课程班：‘8， 10’<br/>显示行政班+课程班：‘1，8， 10’ |
| Key              | 否   | search     | 输入搜索关键字，不展示在业务逻辑中                           |
| IsList           | 否   | true       | 输入框下是否展示搜索结果列表                                 |
| ShowUserType     | 否   | 1          | 班级架构显示人群：<br/>0 // 不显示用户<br/>1 // 班级节点显示 【学生】****<br/>2 // 班级节点显示 【学生，老师】<br/>3 // 班级节点显示 【学生，家长】<br/>4 // 班级节点显示 【学生，家长，老师】<br/>5 // 班级节点显示 【家长】 |
| IsIncludeBranch  | 否   | true       | 是否显示分校                                                 |
| IsOnlyShowBranch | 否   | true       | 是否只显示分校                                               |
| IsAddressBook    | 否   | true       | 通讯录特殊确认参数，异步获取下一步                           |



****

#### 代码示例

```vue
<template>
    <AuiSplitLayout >
        <!-- 1.基础使用方式：默认组件内容为 select-input + addBtn -->
        <AuiChosen 
            ref="chosenRef1"
            :createChosen="chosenMod.createChosen"
            @on-click-ok="handleChosenOk" />
        
        <!-- 2.自定义组件内容：但是需要手动触发选人弹窗方法 -->
    	<AuiChosen 
            ref="chosenRef2"
            :createChosen="chosenMod.createChosen"
            @on-click-ok="handleChosenOk" >
            <!-- 自定义内容区 -->
            <Button type="success" @click="handleAddExist">自定义选人组件formDom</Button>
        </AuiChosen>
        
        <!-- 3.隐式创建，调用时手动触发。场景：按钮组内某个按钮触发选人弹窗 -->
        <Button type="success" @click="handleAddExist">添加成员</Button>
        <!-- 页面中不显示选人组件的同时触发按钮不在选人组件内部 -->
        <AuiChosen 
            ref="chosenRef3"
            v-show="chosenMod.show"
            v-model="chosenMod.value"
            :createChosen="chosenMod.createChosen"
            @on-click-ok="handleChosenOk" />
        
    </AuiSplitLayout>
</template>
<script>

export default {
    data() {
        return {
            chosenMod: {
                show: true,
                // 默认值映射；也可以用于清空已选
                value: [],
                // 选人组件对象参数
                createChosen: {
                    // 选人架构显示参数配置
                    staticParams: {
                        CheckType: 1,
                        DepartmentType: '1,6,4',
                        ShowUserType: (() => {
                            // 高校：行政班学生+家长（高校学生手机号未标识）
                            if (window.app.eduType == 3) {
                                return 3;
                            // 非高校：家长
                            } else {
                                return 5;
                            }
                        })()
                    },
                    total: 1,
                    placeholder: '请选择架构',
                    beforeOkCallback: (arr) => {
                      	// do something
                        return true;
                    }
                }
            }
        };
    },
    methods: {
        handleAddExist () {
            // 手动触发弹窗
            this.$refs.chosenRef.chosenAddFn();
        },
        // 确认回调
        handleChosenOk (rs) {
        	// do something
        }
    }
};
</script>
```

****

#### API  

###### Props

| 属性         | 说明                                                         | 类型   | 默认值 |
| ------------ | ------------------------------------------------------------ | ------ | ------ |
| value        | v-model                                                      | Array  | []     |
| createChosen | 选人组件参数配置。                                           | Object | {}     |
| item         | 单独使用不用考虑。在Aui-form-create中使用item属性用来接受createChosen对象：<br/>type = chosen 展示选人, 触发方式必须是 change,  valid_method需要设置为 ‘’ <br/>[{<br/>      label:  '所属部门',<br/>      key:  'Depart',<br/>      type: 'chosen',<br/>      required:  true,<br/>      trigger: 'change',<br/>      valid_method:  '',<br/>      createChosen:  {<br/>            staticParams: {<br/>                  CheckType: 2,<br/>                  DepartmentType: 6<br/>            },<br/>            placeholder:  '输入搜索部门'<br/>      }<br/>}] | Object | {}     |

###### createChosen

| 属性             | 说明                                                         | 类型           | 默认值  |
| ---------------- | ------------------------------------------------------------ | -------------- | ------- |
| url              | v-model                                                      | String         | []      |
| staticParams     | 选人接口参数对象                                             | Object         | {}      |
| autoParams       | 获取下一级时默认携带的已选对象属性                           | Array          | ['Id']  |
| className        | 自定义其他类名                                               | Array          | []      |
| tipText          | 右侧已选框已选内容提示。默认：‘已选$个架构，$名用户’         | String         | ''      |
| tipRule          | 右侧已选框已选内容提示规则。默认：['IsLeaf', 0, 'IsLeaf', 1] | Array          | []      |
| total            | 最多可选，默认是多选。数值时点击确认有提示                   | String，Number | 'multi' |
| required         | 是否非必选                                                   | Boolean        | false   |
| placeholder      | 输入框默认值。包括输入框搜索，弹窗搜索                       | String         | ''      |
| beforeOkCallback | 确认前回调。通过时 retrue ture 保证回调继续进行，默认参数为已选架构。注意该方法使用时组件内部数量校验失效，需要自定义数量校验 | Function       | -       |

###### Events

| 事件名           | 说明         | 返回值   |
| ---------------- | ------------ | -------- |
| on-select-change | 选人组件输入框内容发生变化后触发 | 已选架构 |
| on-select-del | 选人组件输入框删除已选内容后触发 | 已选架构 |
| on-click-ok | 弹窗组件点击确认后触发 | 已选架构 |

###### Methods

| 事件名      | 说明                                           | 返回值 |
| ----------- | ---------------------------------------------- | ------ |
| chosenAddFn | 触发弹窗方法。默认点击添加Icon触发，可手动调用 | 无     |