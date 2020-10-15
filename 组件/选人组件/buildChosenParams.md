

#### 概述   

```markdown
 aui-chosen 静态参数集中管理方法, 因为选人组件参数区分中小学/高校, 使用版本类型只会让代码可读性降低，所以开发了参数管理脚本。
```

****

#### main.js

```javascript
/**
 * @author zxd
 * @module 全局挂载选人参数
 */
// 引入选人组件静态参数方法
import buildChosenParams from '@/views/school/chosen_staticParams';
app.buildChosenParams = buildChosenParams;
```

------

#### school/chosen_staticParams.js

```javascript
/**
 * @author zxd
 * @module 教职工管理 选人组件参数管理
 * @date 2019/11/08
 * @params scene: 场景值
 * @params eduType: 直接传入后台返回的教育类型 => 学校版本类型：3是高校；1，2为中小学
 */
import Student from './classes/_chosen_staticParams';

export default (scene, eduType) => {
    const chosenConfig = {
        ...Student,
        ...Teacher
    };

    // 选人组件参数有教育类型之分，需要根据不同教育类型返回
    if (eduType) {
        // 学校版本类型：3是高校；1，2为中小学
        const eduTypeName = eduType == 3 ? 'colleges' : 'middle';
        console.log(chosenConfig[scene][eduTypeName]);
        return chosenConfig[scene][eduTypeName] || false;
    }
    
    // 无教育类型根据场景值返回
    return chosenConfig[scene] || false;
};
```
#### classes/_chosen_staticParams.js

```javascript
/**
 * @author zxd
 * @module 班级管理 选人组件参数管理
 * @date 2019/11/08
 */
export default {
    // 选择已有为学生
    existStudent: {
        // 中小学没有选择已有为学生, 定义定义为null，use.vue 通过返回值可以判断隐藏
        middle: null,
        colleges: {
            CheckType: 1,
            DepartmentType: '1,2,5,6',
            ShowUserType: 5
        }
    },
    existParents: {
        middle: {
            CheckType: 1,
            DepartmentType: '1,2,5,6',
            ShowUserType: 5
        },
        colleges: {
            CheckType: 1,
            DepartmentType: '1,2,4,5,6',
            ShowUserType: 3
        }
    }
};
```
------

#### Uses

```vue
<script>
export default {
    data () {
        return {
            createChosen: {
                // 有教育类型的需要传入教育类型
                staticParams: app.buildChosenParams('existStudent', window.app.eduType),
                total: 1,
                placeholder: 'xxxxxx'
            }
        }
    }
}
</script>
```

