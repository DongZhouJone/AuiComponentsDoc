#### 概述   

```markdown
 Aui-split-layout 基于 iview Split + Spin + AuiLayout 封装。
 优点：1. 化iview Split分割方式为 flex, 修复iview使用定位导致的脱离文档流, 参考组件: Aui-split-horizontal + Aui-split-			 trigger + aui-split-dom.js
      2. 优化AuiLayout插槽为自动检测, 无需配置
      3. 优化网络错误页面展示
      4. 页面滚动全屏滚动
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
    <template slot="content"></template>
    <!-- 右侧内容区-标题栏 -->
    <template slot="title"></template>
    <!-- 右侧内容区-右侧分栏(30%)-->
    <template slot="right"></template>
    <!-- 右侧内容区-页脚-->
    <template slot="footer"></template>
    <!-- 自定义右侧内容区遮罩内容 layout.error 控制 -->
    <template slot="err-cont">
        <img src="./img/course_class-empty.png" alt="">
        <p class="mt10 c-999">请您先在左侧添加课程班级,选择班级后管理并查看班级成员~</p>
    </template>
</AuiSplitLayout>
```

###### script

```javascript
data() {
    return {
        layout: {
            className: pageName,
            loading: false
        }
    };
},
methods: {
    // 假设这个是个获取详情的函数，需要加载动画
    async fn () {
        let resp;
        
        this.layout.loading = true;
        try {
        	resp = await app.get(url, params);
            if (resp.ErrorCode === app.global.ErrorCode) {
                this.layout.loading = false;
                // do something
			} else {
                // 需要全局引入 app.mixin
                // 看业务参加，例如获取编辑详情，获取失败执行该方法，默认展示为 '网络开小差'
                this.showError();
            }
        } catch (e) {
            // 2.0 优化ajax错误码返回逻辑，404，500 均可以捕捉，该方法会根据 e.status 显示 layout 对应的错误信息页
            this.showError(e);
		}
    }
}
```

###### src/mixins

```javascript
// 配合 Aui-split-layout 使用的混合函数
showError (error) {
    this.layout.loading = false;
    this.layout.error = true;
    this.layout.errorCode = '';
    if (error && error.status) {
        this.layout.errorCode = error.status;
    }
    if (error && error.code && error.code != 0) {
        this.$Message.error({
            content: error.msg,
            duration: 10,
            closable: true
        });
    }
}
```

------

#### API  

###### Props

| 属性             | 说明                                                         | 类型          | 默认值 |
| ---------------- | ------------------------------------------------------------ | ------------- | ------ |
| layout           | 页面配置对象。使用布局组件配置对象应该在Vue.data中顶置       | Object        | {}     |
| layout.className | 页面命名空间类，推荐每个页面都写                             | String，Array | ''     |
| layout.loading   | 页面loading状态，只覆盖content区，常用于编辑页面获取数据过渡 | Boolean       | false  |
| layout.error     | 可手动控制的显示异常页面，没有错误码时可以自定义错误内容     | Boolean       | false  |
| layout.errorCode | 支持 403，404，500，其他状态统一为：网络开小差               | String        | ''     |
| splitValue       | 分隔面板左边宽度，同时也是分隔区域的最小宽度，必须是 ‘numpx’ | String        | ‘0px’  |

###### Methods

| 方法明 | 说明                             | 返回值 |
| ------ | -------------------------------- | ------ |
| reload | 简单封装window.location.reload() | 无     |

###### Slot

| 名称     | 说明：插槽标记推荐使用 <template slot="slotName"></template> |
| -------- | ------------------------------------------------------------ |
| tree     | 页面左侧内容区，常用于放组织架构树                           |
| content  | 页面右侧内容区，常用展示表格，loading, error 区只覆盖右侧内容区 |
| title    | 右侧内容title，只展示标题时*左对齐*，同时有返回按钮时标题*右对齐* |
| right    | content 区右侧区域，通常用来展示 **审核** 业务               |
| footer   | content区页脚，表单编辑页提交按钮组。2.0 项目统一右置        |
| err-cont | layout.error = true 时，可以自定义错误信息页。参考课程班只有激活班级节点时展示页面功能 |