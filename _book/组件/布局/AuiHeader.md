#### 概述   

```markdown
 Aui-header 基于 iview Menu + Dropdown 封装头部导航组件。
 优点：感觉没啥优点，持续开发中
```

****

#### 代码示例

##### viewPage

```vue
<template>
	<AuiHeader class="aui-layout-home-header js-head"
               :loginModel="loginModel" 
               @clearKeepAlive="_clearKeepAlive"
               @changeSystem="_changeSystem"/>
</template>
<script>
    export default {
    data() {
        return {
            // 用户登录信息，头部展示
            loginModel: {}
        };
    },
    beforeMount() {
        // 去除全屏loading
        this.$nextTick(() => {
            this.removeDom();
        });
        this.login();
    },
    mounted() {
    },
    methods: {
        removeDom () {
            document.getElementById('homeSkeleton').remove();
        },
        async login () {
            var resp = await app.get('login.getLoginInfo');
  
            if (resp.ErrorCode === app.global.ErrorCode) {
                // 全局保存登录信息
                window.app.loginInfo = JSON.parse(JSON.stringify(resp.Data));
                // 缓存保留登录信息
                this.$storage.localset('views.home.loginInfo', resp.Data);
            } else {
                if (window.app.env == 'online') {
                    window.app.redirect();
                }
            }
        },
        _clearKeepAlive () {
            this.keepAliveList = [];
        },
        _changeSystem () {
            this.login();
        }
    },
    components: {
        AuiHeader
    }
};
</script>
```

****

#### API  

###### Props

| 属性             | 说明                                        | 类型    | 默认值 |
| ---------------- | ------------------------------------------- | ------- | ------ |
| loginModel       | 用户登录信息对象; {Admin:{}, MenuList: []}; | Object  | {}     |
| hideChangeSystem | 是否隐藏平台切换。true时不请求可切换列表    | Boolean | false  |

###### Events

| 事件名         | 说明                                 | 返回值 |
| -------------- | ------------------------------------ | ------ |
| clearKeepAlive | 用于清空keep-alive方法（目前已作废） | 无     |
| changeSystem   | 平台切换事件                         | 无     |

###### Methods

| 事件名      | 说明                                            | 参数     | 返回值 |
| ----------- | ----------------------------------------------- | -------- | ------ |
| app.setMenu | 手动控制菜单高亮的方法。参数为后台返回menu.Menu | menuName | -      |



