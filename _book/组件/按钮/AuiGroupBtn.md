#### 概述   

     Aui-group-btn  基于 iview Button + Dropdown 封装二维按钮列表。
     优点：根据配置自动按钮组，可以服务端返回。区别于1.0，无需考虑样式问题。
     缺点：暂时没啥缺点。我把隐藏按钮组的方法只写入mixin, 为了避免按钮先全部渲染后隐藏造成闪烁
****
#### 代码示例

##### viewPage

```javascript
let list = [
    { // 普通按钮
        "Id": "1",
        "Name": "添加学生"
    },
    { // 复合按钮
        "Id": "2",
        "Name": "批量操作",
        "Child": [{
            "Id": "2_1",
            "Name": "删除"
        },
        {
            "Id": "2_2",
            "Name": "调班"
        }]
    }
];

// @on-click callback
groupBtnClickHandler (item) {
    // 组件内部做了兼容大小写，推荐首字母大写，不清楚请打印下item
    switch (item.id) {
        case '1':
            // do something
            // do something
            // do something
            break;
    }
}
```

##### src/mixins
```javascript
// getBtnGroup: 获取按钮组 @params: {PageCode: 'classes', DepartmentId: id} 
// 该方法返回一个promise, ().then(resp => {});
async getBtnGroup (paramsObj) {
    let params = Object.assign({}, {
        PageCode: 'classes'
    }, paramsObj);
    let btnList = [];

    let resp = await app.get('common.getButton', params);
    if (resp.ErrorCode === app.global.ErrorCode) {
        btnList = resp.Data.Data;
    }
    console.log('parsss', btnList);
    return btnList;
},
// 隐藏按钮组一个或多个按钮 @params: hideIds => String/Array 
// 生成一个新的按钮组，直接覆盖当前项目中的btnList
hideBtnList (btnList, hideIds) {
    if (!Array.isArray(btnList)) return;
    if (!Array.isArray(hideIds)) hideIds = [hideIds];
    let arr = JSON.parse(JSON.stringify(btnList));

    arr.forEach(n => {
        if (hideIds.indexOf(n.Id) != -1) {
            n.isHide = true;
        }
        if (n.Child && n.Child.length) {
            n.Child.forEach(m => {
                if (hideIds.indexOf(m.Id) != -1) {
                    m.isHide = true;
                }
            });
            // 只有一项是隐藏父级按钮
            if (n.Child.length == 1 && n.Child[0]['isHide']) {
                n.isHide = true;
            }
        }
    });

    return arr;
}
```

****
#### API  

###### Props

| 属性 | 说明                                  | 类型  | 默认值 |
| ---- | ------------------------------------- | ----- | ------ |
| list | 生成按钮组数据源, 为空时有占位Dom生成 | Array | []     |

###### Events

| 事件名   | 说明         | 返回值                   |
| -------- | ------------ | ------------------------ |
| on-click | 按钮点击事件 | 返回当前按钮所有数据信息 |

###### list-item

| 属性     | 说明                                         | 类型    | 默认值 |
| -------- | -------------------------------------------- | ------- | ------ |
| Id       | 按钮唯一标识                                 | String  | -      |
| Name     | 按钮名称                                     | String  | ''     |
| isHide   | 是否隐藏该按钮                               | Boolean | false  |
| disabled | 是否不可用                                   | Boolean | false  |
| Child    | 子按钮，以下拉列表的形式展示。这时父按钮无效 | String  | ''     |





