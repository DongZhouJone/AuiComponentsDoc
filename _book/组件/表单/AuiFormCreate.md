

#### 概述 

```markdown
 aui-form-create 基于 iview 所有表单组件封装的自动化创建的表单，多用于表单页。
 优点：根据配置自动生成表单，校验，返回值
 缺点：因为开发周期短，create内部单个组件使用没有写单独使用功能，导致组件在create内使用简单，单独使用可能比较麻烦
```
****

#### API   

###### Props

| 属性       | 说明                                           | 类型    | 默认值 |
| ---------- | ---------------------------------------------- | ------- | ------ |
| labelWidth | 表单Label宽度                                  | Number  | 120    |
| inline     | 表单项是否内联展示, 默认为换行展示             | Boolean | false  |
| hideLabel  | 隐藏label项                                    | Boolean | false  |
| row        | label占满一行展示                              | Boolean | false  |
| disabled   | 表单disabled状态，状态权重高于单项disabled配置 | Boolean | false  |
| formConfig | 动态创建表单的配置项, 空数组时隐藏表单         | Array   | []     |
| formModel  | 表单model，用于form赋值默认值                  | Object  | {}     |

###### Events

| 事件名    | 说明                                           | 返回值                |
| --------- | ---------------------------------------------- | --------------------- |
| on-change | 表单model发生变化时，返回整个更新后的formModel | 数据改变后的formModel |

###### Methods

| 方法明      | 说明                                                         | 参数 | 返回值  |
| ----------- | ------------------------------------------------------------ | ---- | ------- |
| valid       | 表单自动校验，需要配合validate.js使用,没有直接的返回值。返回值需要在父级页面调取 this.$refs.auiFormCreate.isValid | 无   | Boolean |
| resetFields | 清空表单校验状态                                             | 无   | 无      |

###### Slot

| 名称 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| key  | slot名对应formConfig中key值, 当复杂组件无法用aui-form-create实现时，在父级页面可以定义<br/><template slot="slotName">dom</template> |
****

#### formConfig 

formConfig-item|   | |
---|------------|---|---
属性 | 说明 | 类型 | 默认值    
label | 表单label | String | - |
key | 表单提交key, formModel内唯一，在自定义组件中用于确定componentsName使用 | String | - |
type | 表单类型：text, date, time, radio, checkbox, img, identity, area, select, chosen, chosen_depart, article。不同的type会有特殊的附加属性 | String | '' |
required | 是否必填，为true显示label 红星号  | Boolean | false |
placeholder | 输入框默认值值 | String | - |
showMaxLen | 是否显示输入框后auiMaxLength组件 | Boolean | false |
maxLen | auiMaxLength 最大字数限制, showMaxLen开启时默认为30，可用不用设置 | Number | 30 |
disabled | 单项表单disabled状态，权重低于aui-form-create的disabled = true | Boolean | false |
valid_method | 基于 validate.js 的表单校验方法，auiFormCreate.validate()会执行每项表单的该方法，校验通过时才会返回true | String | '' |
trigger | 触发表单校验事件focus, blur, change | String | blur |
extra | 额外的说明，目前与表单内联，可以继续优化 | String | '' |
list | type类型为radio, checkbox, select时可以已自定义数据源, 其他类型时无效。格式：[{Label: abc, Value: Id, Disabled: false}] | Array | [] |
vertical | type类型为radio, checkbox时用来控制选项横排竖排，默认为横排。值为'1'时为竖排 | String,Number | 0 |
className | type类型为text, select时可以定义特殊类来控制输入框长度 | String|Array | - |

###### type: text

| 属性       | 说明                                                         | 类型   | 默认值 |
| ---------- | ------------------------------------------------------------ | ------ | ------ |
| input_type | type为text时, 可使用 input_type 设置为 textarea, 多行文本框  | String | 'text' |
| minRows    | input_type 设置为 textarea，多行文本框最小行数               | Number | 5      |
| maxRows    | input_type 设置为 textarea，多行文本框最大行数               | Number | 7      |
| reg        | valid_method 为 Text 类型时，可以自定义正则。例：/^[\da-zA-Z]{1,32}$/ | RegExp | -      |
| msg        | valid_method 为 Text 类型时，可以自定义校验失败提示信息。例：支持字母与数字混合 | String | ''     |

###### type: select

| 属性 | 说明                                                         | 类型   | 默认值 |
| ---- | ------------------------------------------------------------ | ------ | ------ |
| key  | 2.0采用全局一次返回AllSelect, 当key和返回的key对应时会直接把AllSelect属性作为数据源 | String | ''     |

###### type: chosen

| 属性         | 说明                    | 类型   | 默认值 |
| ------------ | ----------------------- | ------ | ------ |
| createChosen | 2.0 aui-chosen 参数配置 | Object | {}     |


###### type: area
| 属性        | 说明                                                         | 类型    | 默认值 |
| ----------- | ------------------------------------------------------------ | ------- | ------ |
| showCountry | 是否显示国家                                                 | Boolean | false  |
| errMsg      | require = true时自定义错误信息。事实是当时开发太多组件了，没有优化。。。 | String  | ''     |

###### type: date/time 
*非完全封装iview, 需要更多参数可以找我*
| 属性      | 说明                                                         | 类型   | 默认值 |
| --------- | ------------------------------------------------------------ | ------ | ------ |
| date_type | 日期类型，可选值为 date、daterange、datetime、datetimerange、year、month | String | ''     |
| time_type | 显示类型，可选值为 time、timerange                           | String | ''     |

###### type: img 
*非完全封装iview, 需要更多参数可以找我*
| 属性   | 说明                                                         | 类型   | 默认值 |
| ------ | ------------------------------------------------------------ | ------ | ------ |
| config | 上传图片配置，有默认配置，可覆盖性修改。只支持单图上传，多图的看大家了 | Object | {}     |

```javascript
// upload-config
{
    url: window.app.global.uploadUrl, // 图片上传地址，默认为https://m.campus.qq.com/api/upload/create
    area: [IMAGE_WIDTH, IMAGE_HEIGHT], // 图片大小，默认为一寸照片.5倍
    format: ['jpg', 'jpeg', 'png', 'bmp'],
    maxSize: 2048,
    outShowMsg: false, // 图片信息是否内部显示，true时和图片组件内联排列
    msg: '请上传标准1寸照，支持jpg、jpeg、png、bmp，单张2M以内' // 错误信息
}
```
****

#### validate.js
*所有表单非空校验根据 label自动生成*

###### Methods

| 名称                 | 说明                                                       |
| -------------------- | ---------------------------------------------------------- |
| Text                 | 文本校验，支持自定义正则reg                                |
| Name                 | 姓名校验，统一规则                                         |
| Radio                | 单选，建议model为字符串，aui-form-create会做次内部类型转换 |
| Checkbox             | 多选，必须为数组                                           |
| Mail                 | 邮箱校验，统一规则                                         |
| Phone                | 座机电话校验，区号+座机号码+分机号码                       |
| CellPhone            | 移动手机校验，(+86区号)                                    |
| AllPhone             | 座机+移动手机                                              |
| Url                  | url校验，https/http强校验                                  |
| ChiEng               | 只支持中文字符大小写英文字符，可通过text时reg自己拓展      |
| App                  | 只支持中文字符大小写英文字符及数字                         |
| AlphaUnderLine       | 只支持大小写英文字符及下划线                               |
| AlphaChiUnderLine    | 只支持中文字符大小写英文字符空格及下划线                   |
| AlphaSlash           | 只支持大小写英文字符及斜杠                                 |
| AlphaUnderLineNotTop | 只支持大小写英文字符及下划线，且不能以下划线开头           |
| AccountCard          | 中华人民共和国身份证                                       |
| OfficerCard          | 中华人民共和国军官证                                       |
| PassPortCard         | 护照                                                       |
| HKCard               | 港澳居民身份证                                             |
| TWCard               | 台湾居民来往大陆通行证                                     |

