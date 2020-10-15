#### 概述

     基于 iview Modal + cropperjs 封装。
     优点：黑盒封装，知道怎么用就行。
     缺点：可能产品会不喜欢

[cropperjs 官方文档](http://fengyuanchen.github.io/cropperjs)   
[cropperjs 中文文档](https://blog.csdn.net/weixin_38023551/article/details/78792400)
****
#### 代码示例

弹窗展示（也可以在页面直接使用）
```javascript
// template
<Modal
    v-model="uploadMod.show"
    title="裁剪上传Banner图"
    width="640"
    footer-hide
    :mask-closable="false"
    :loading="uploadMod.loading">
        <div class="cropper-example cropper-first" v-show="uploadMod.show">
            <cropper
                ref="cropperRef"
                :options="cropperOptions"
                :optionsBtnGroup="optionsBtnGroup"
                :src="exampleImageSrc"
                crop-button-text="确认"
                @on-crop="handleCroped"/>
        </div>
</Modal>

// methods
_getUUID () {
    var d = new Date().getTime();
    var uuid = 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        var r = (d + Math.random() * 16) % 16 | 0;
        d = Math.floor(d / 16);
        return (c === 'x' ? r : (r & 0x7 | 0x8)).toString(16);
    });
    return uuid;
},
async handleCroped (blob) {
    let uid = this._getUUID();
    const formData = new FormData();
    blob.name = uid + 'circle.png';
    formData.append('Content-Type', blob.type || 'application/octet-stream');
    formData.append('file', blob, blob.name);
    formData.append('type', 3);
    
    // window.app.global.uploadUrl = 'https://m.campus.qq.com/api/upload/create'
    let resp = await app.post(window.app.global.uploadUrl, formData);
    this.$refs.cropperRef.endUploading();
    if (resp.code == 0) {
        this.$Message.success('上传成功');
        this.miniCover = resp.data.url;
        this.uploadMod.show = false;
        this.exampleImageSrc = '';
    }
}
```
****
#### API  

###### Props

| 属性            | 说明                                                         | 类型   | 默认值 |
| --------------- | ------------------------------------------------------------ | ------ | ------ |
| src             | 被裁剪图片地址                                               | String | ''     |
| moveStep        | 移动距离基数                                                 | Number | 4      |
| cropButtonText  | 确认按钮文案                                                 | String | '裁剪' |
| optionsBtnGroup | 裁剪简快捷键操作按扭组。默认值: <br/> [  <br/>'rotate', 'shrink', 'magnify', <br/> 'shuipingfanzhuan', 'up', 'chuizhifanzhuan', <br/> 'left', 'down', 'right', <br/> ] | Array  | []     |
| options         | cropperjs官方参数扩展                                        | Object | {}     |

###### Events

| 事件名  | 说明         | 返回值                 |
| ------- | ------------ | ---------------------- |
| on-crop | 裁剪成功回掉 | 返回裁剪图片base64格式 |

###### Methods

| 事件名       | 说明                                          | 返回值 |
| ------------ | --------------------------------------------- | ------ |
| replace      | 清空src属性时用到，需要用这个方法重载crop实例 | 无     |      |
| endUploading | 上传成功后需要手动关闭上传loading态           | 无     |      |