#### 概述   

```markdown
 aui-collapse-transition.js 展开收起动画组件,  是函数式组件
 优点： 不需要考虑内容区高度问题, 简单的展开收起动画方法, 增强用户体验
 缺点： 没有做成全局组件, 需要单独引入使用
```

------

#### 代码示例

##### viewPage

###### template

```html
<!-- 展开收起触发按钮 -->
<span class="options f-r">
    <Button v-show="!show" icon="ios-arrow-down" type="text" @click="toggleSiled()">展开</Button>
    <Button v-show="show" icon="ios-arrow-up" type="text" @click="toggleSiled()">收起</Button>
</span>
<!-- 动画组件包裹需要做动画的内容区 -->
<collapse-transition>
	<!-- 动画组件包裹需要做动画的内容区 -->
    <div class="aui-collapse-cont-wrap" v-show="show">
        xxxxx
    </div>
</collapse-transition>
```

###### script

```javascript
import CollapseTransition from './aui-collapse-transition';
export default {
    name: 'AuiCollapse',
    components: {
        CollapseTransition
    },
    data () {
        return {
            // 内容区必须要有显示隐藏的过程
            show: false
        };
    },
    methods: {
        toggleSiled () {
            this.show = !this.show;
        }
    }
};
```

#### 原理

当有内容的元素收起时，虽然高度未知，但是**滚动高度** 是固定的，通过获取 滚动高度复制 height, 实现动画过程。有兴趣的同学可以参考代码：base\src\components\layout\aui-collapse-transition.js