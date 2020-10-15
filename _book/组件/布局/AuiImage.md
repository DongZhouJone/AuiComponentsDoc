#### 概述   

```markdown
 aui-image 是图片展示组件
 优点： 图片不拉伸撑满容器, 原生封装图片异常处理, 不会出现图裂的情况
 缺点： 没有做成全局组件, 需要单独引入使用; 功能可能不全, 需要大家提更多业务场景
```

------

#### 代码示例

##### viewPage

###### template

```vue
<template>
    // 单图用法
    <AuiImage class="cursor mr5 mb5" :src="item.Pic[0]" radius="0" followPic @on-click="handlePreview"/>

    // 朋友圈9宫格图片展示
    // 9宫格图片展示父级容器需要固定宽度，长度约为 3张图的宽，自动换行
    // 配合图片预览组件使用：因为@on-click只返回单图地址，需要定义过渡函数
	// 这里图片预览用viewer
    <div class="pyq-item-photos" v-for="(pic, picIdx) in item.Pic" :key="picIdx">
        <AuiImage class="mr5 mb5" :src="pic" width="105" radius="0"  @on-click="(img) => {
            return previewPhoto(item.pic, picIdx);
         }"/>
    </div>
</template>
<script>
export default {
    methods: {
        // 参数：list 图片列表；idx 图片下标
        previewPhoto (list, idx) {
            // 这个组件我改过，可以这么用
            var viewer = new this.$viewer(list, {
                hidden: function () {
                    viewer.destroy();
                }
            });
            viewer.view(idx);
        }
    }
}
</script>
```

------

#### API   

###### Props

| 属性      | 说明                                                         | 类型    | 默认值 |
| --------- | ------------------------------------------------------------ | ------- | ------ |
| src       | 图片地址                                                     | String  | ''     |
| width     | 图片宽度                                                     | String  | auto   |
| height    | 图片高度，默认等于宽度                                       | String  | ''     |
| radius    | 图片圆角，默认4px                                            | String  | 4      |
| color     | 图片容器背景，默认rgba(0,0,0, .3)                            | String  | ''     |
| followPic | 类似朋友圈单图展示方案，需要父级容器固定宽度；详情：图宽 >= 容器宽 ？容器宽 ：图宽，高度不限 | Boolean | false  |

###### Events

| 事件名   | 说明                         | 返回值  |
| -------- | ---------------------------- | ------- |
| on-click | 图片点击事件，常用于图片预览 | 图片url |