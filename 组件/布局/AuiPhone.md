#### 概述   

```markdown
 aui-phone 手机预览组件 
 优点： 只需要考虑手机内容区, 减少页面排版时间
 缺点： 目前只支持iPhone6, iPhoneX. 后期按需求可以增加其他型号
```

------

#### 代码示例

##### viewPage

###### template

```html
<AuiPhone :version="x"/>
```

------

#### API   

###### Props

| 属性    | 说明              | 类型   | 默认值 |
| ------- | ----------------- | ------ | ------ |
| version | 手机版本号：6， x | String | '6'    |

