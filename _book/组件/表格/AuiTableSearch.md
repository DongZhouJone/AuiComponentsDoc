#### 概述   

```markdown
 Aui-table-search 基于 iview Input + Select + Button 二次封装。
```

------

#### 代码示例

##### viewPage

###### template

```html
<table-search placeholder="请输入姓名/学号/手机号/卡号搜索" @search="handleSearch"></table-search>
```

###### script

```javascript
methods: {
    handleSearch (value, type) {
        this.freshTable({'Key': value}, 'search');
    }
}
```

#### API  

###### Props

| 属性        | 说明                                                         | 类型    | 默认值   |
| ----------- | ------------------------------------------------------------ | ------- | -------- |
| placeholder | 输入框默认提示语                                             | String  | '请输入' |
| prependList | input前增加下拉选框，在后台不支持多条件符合搜索时可以用。有值时展示 | Array   | []       |
| showBtn     | 是否显示搜索按钮                                             | Boolean | false    |

###### Event

| 方法明 | 说明                    | 返回值         |
| ------ | ----------------------- | -------------- |
| search | 点搜索icon 或 按钮 触发 | 输入搜索关键字 |
