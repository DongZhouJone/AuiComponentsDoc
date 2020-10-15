

#### 概述   

```markdown
 aui-bread-crumb 基于 iview Breadcrumb + BreadcrumbItem 二次封装的面包屑导航 
 优点： 全局组件, home.vue 已经引入, 根据路由 meta.list 动态生成
 缺点： 需要手动配置
```

------

#### 代码示例

##### viewPage

###### template

```html
<AuiBreadCrumb :fontSize="14"/>
```

###### script

```javascript
const routers = [
    {
        path: 'classes',
        name: 'classes',
        meta: {
            root: 'classes',
            title: '班级管理',
            menu: 'classes'
        },
        component: Index
    },
    {
        path: 'classes/add/:id?',
        name: 'classes_add',
        meta: {
            root: 'classes',
            title: '添加学生&家长',
            // 动态的二级和二级以上菜单需要函数式返回，避免页面刷新后面包屑显示异常
            list: (params) => {
                return [
                    {
                        name: 'classes',
                        title: '班级管理',
                        path: '/classes'
                    }, 
                    // 新增时无id，可以用来缺点不同类型title
                    {
                        name: 'classes_add',
                        title: !params.id ? '添加学生' : '编辑学生信息',
                        path: ''
                    }
                ];
            }
        },
        component: AddStudent
    },
    {
        path: 'classes/checkInfo',
        name: 'checkInfo',
        meta: {
            root: 'classes',
            title: '审核',
            // 固定二级页面可以写固定数组, 并且第二级可以不写path
            list: [
                {
                    name: 'classes',
                    title: '班级管理',
                    path: '/classes'
                }, 
                {
                    name: 'checkInfo',
                    title: '学生信息审核',
                    path: ''
                }
            ]
        },
        component: AddStudent
    }
];
```
------
#### API   

| 属性  | 说明                                  | 类型   | 默认值 |
| ----- | ------------------------------------- | ------ | ------ |
| name  | 建议和路由name保持一致                | String | ''     |
| title | 页面标题，面包屑中文标题              | String | ''     |
| path  | 面包屑跳转需要用到 path, 即route.path | String | ''     |

