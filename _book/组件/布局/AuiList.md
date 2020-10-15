#### 概述   

```markdown
 aui-list 类似表单 label-input 展示, 常用于纯展示文本类详情 
 优点： 支持多种类型展示, 避免多余的样式开发
 缺点： 一年前开发的组件, 不一定完全适用 2.0, 可以合理进行二次开发
```

------

#### 代码示例

##### viewPage

###### template

```html
<div v-for="(item, index) in list" :key="index">
    <AuiList :item="item" :width="120"></AuiList>
</div>
```
------

#### API   

###### Props

| 属性      | 说明                               | 类型     | 默认值 |
| --------- | ---------------------------------- | -------- | ------ |
| item      | 数据源,  例如：{ label: '', value: '', type: 'text' } | Object   | {}  |
| align     | label 文字内容对齐方式，可选 right | String| left |
| width     | label 宽度                 | Number| 100 |
| linewidth | label-item 行高  | Number | 10 |

###### Props-item

*坑爹的 2.0 用首字母大写数据， item 数据源最好前端进行二次处理*

| 属性   | 说明                                                         | 类型                      | 默认值 |
| ------ | ------------------------------------------------------------ | ------------------------- | ------ |
| label  | 列表项 label                                                 | String                    | -      |
| value  | 列表项 具体内容                                              | String, Array, 字符串对象 | -      |
| type   | 类型：text, radio, checkbox, table, json, img, pre-text(代码块) | String                    | text   |
| list   | type = radio, checkbox 时支持传入列表                        | Array                     | []     |
| header | type = table 时，支持传入表头                                | Array                     | []     |

