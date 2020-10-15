#### 概述   

```markdown
 Aui-table 基于 iview Table + Page 二次封装。
 优点：
 	  1. 参考 1.0 bootstrapTable设计思路封装, 使用更方便
      2. 组件已封装布局，无需考虑样式问题
      3. 组件内部自动执行获取表格。优化【表格删除列表导致暂无数据】的痛点
      4. 统一表格规范，操作列为动态返回
```

------

#### 代码示例

##### viewPage

###### template

```html
<AuiSplitLayout :layout="layout" splitValue="275px" >
    <!-- 左侧树插槽 -->
    <template slot="tree"></template>
    <!-- 右侧内容区 -->
    <template slot="content">
        <AuiTable
            ref="tableRef"
            :createTable="createTable"
            @on-table-callback="tableCallback"
            @on-table-selection-change="tableSelectionCallback"">
            <template slot="table-hd-left">
                <AuiGroupBtn :list="btnList" @on-click="groupBtnClickHandler"/>
            </template>
            <template slot="table-hd-right">
                <table-search :keyword="keyword" placeholder="输入搜索" @search="handleSearch" />
            </template>
        </AuiTable>    
    </template>
</AuiSplitLayout>
```

###### script

```javascript
data() {
    return {
        createTable: {
            columns: this.getColumns(),
            url: 'class.studentList'
        }
    };
},
methods: {
    // 表头输入搜索框
    handleSearch (value, type) {
        this.freshTable({'Key': value}, 'search');
    },
    // 刷新表格
    // 1. 首次执行 this.freshTable({DepartmentId: node.data.Id}, 'fresh', true);
    // 2. 保留参数刷新 this.freshTable()， 但是不会刷新表头; 
    // 3. 删除刷新 this.freshTable(null, 'del'), 内部兼容删除最后一条数据向前退一页;
    freshTable (params, flag, isTrigger) {
        this.$refs.tableRef.freshTable(params, flag, isTrigger);
    },
    // 刷新表头
    freshTableHeader () {
        this.createTable.columns = this.getColumns();
    },
    // 动态获取当前表格 params
    getTableParams () {
        return this.$refs.tableRef.getParams();
    },
    getColumns () {
        // filters 配置只能同步获取，这个方法 this 指向有点问题
		return [
            {
                type: 'selection',
                width: 60
            },
            {
                title: '姓名',
                key: 'Name',
                minWidth: 120,
                isHide: true,
                filters: [
                    {
                        label: '正常',
                        value: '1'
                    },
                    {
                        label: '锁定',
                        value: '4'
                    }
                ],
                filterMultiple: false,
                filterRemote: (value, row) => {
                    this.freshTable({Status: value[0] || 0}, 'filter');
                },
                render: (h, params) => { 
                }
            },
            {
                title: '学号',
                key: 'UserNo',
                minWidth: 150
            },
            {
                title: '操作',
                key: 'Opts',
                fixed: 'right',
                width: 210,
                render: (h, params) => {
                    var opt = params.row.Opts;
                    var doms = [];
                    opt.map((val, index) => {
                        switch (val) {
                            case 'edit':
                                doms.push(h('a', {
                                    attrs: {
                                        title: '编辑'
                                    },
                                    class: '',
                                    on: {
                                        'click': (val) => {
                                            this.fn1(params.row.Id);
                                        }
                                    }
                                }, '修改'));
                                break;
                            case 'del':
                                doms.push(h('a', {
                                    attrs: {
                                        title: '删除'
                                    },
                                    class: 'sc-red',
                                    on: {
                                        'click': (val) => {
                                            this.fn2([params.row.OrgUserId], params.index);
                                        }
                                    }
                                }, '删除'));
                                break;
                        }
                    });
                    if (doms.length) {
                        return h('span', {class: 'aui-table-opts'}, doms);
                    } else {
                        return h('span', '-');
                    }
                }
            }
        ];
    }
}
```

------

#### 表格注意项

1. 2.0 项目操作栏右侧固定定位，表格全屏滚动
2. 除操作栏表格均需要设置 *minWidth*，使每列均右默认最小宽度，屏幕过大也能适应
3. 状态栏字体颜色判断禁止使用判断语句，推荐使用对象定义 state: color
4. Opts操作栏采用动态返回渲染按钮的办法。这里的动态判断 switch 无 default **!!!**  正向操作按钮为 绿色（sr-green)，警示性操作为橘色（sc-orange), 删除类操作为红色（sc-red)。按钮外层需要用 .aui-table-opts 包裹，避免类污染

------
#### API  

###### Props：createTable 表格配置参数

| 属性                | 说明                                     | 类型     | 默认值 |
| ------------------- | ---------------------------------------- | -------- | ------ |
| url                 | 表格请求地址                             | String   | ''     |
| columns             | 表头配置                                 | Array    | []     |
| codeSuccessCallback | 预处理表格列表方法，默认直接返回DataList | Function | -      |

###### Props：createTable.columns 表头配置

| 属性                                                         | 说明                                          | 类型    | 默认值 |
| ------------------------------------------------------------ | --------------------------------------------- | ------- | ------ |
| [iview-table-column](https://www.iviewui.com/components/table#column) | 基础参数参考 iview                            | Array   | []     |
| isHide                                                       | 隐藏列表某一列时设置为 true。这里可以继续优化 | Boolean | false  |

###### Events

| 方法明                    | 说明                               | 返回值        |
| ------------------------- | ---------------------------------- | ------------- |
| on-table-callback         | 表格获取后回调                     | 返回列表total |
| on-table-selection-change | 表格复选框选中状态发生变化时回调。 | 返回已选列表  |

###### Methods

| 方法明     | 说明                                   | 参数                                                         | 返回值       |
| ---------- | -------------------------------------- | ------------------------------------------------------------ | ------------ |
| freshTable | 表格刷新，常用封装方法在代码示例模块   | params： 搜索表格参数eg: {OrgId: 1} <br/>flag：  fresh 刷新表格， del 删除 <br/>isTrigger： 是否触发table loaded Callback事件，目前仅仅用于显示组织架构人数 | 无           |
| getParams  | 获取当前状态列表的ajax参数，常用于导出 | 无                                                           | 返回请求参数 |

###### SlotSlot

| 名称           | 说明：插槽标记推荐使用 <template slot="slotName"></template> |
| -------------- | ------------------------------------------------------------ |
| table-hd-left  | 表头左侧区域。常用来放 **按钮组**                            |
| table-hd-right | 表头右侧区域。常用来放 搜索框                                |

