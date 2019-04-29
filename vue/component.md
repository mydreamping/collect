全局使用组件

方法一：

```
1.在main.js中正常的引入组件

eg：import DropDownRefresh from "./components/DropDownRefresh"; 

2.Vue.component(组件名,{方法})

eg： 

Vue.component("DropDownRefresh", DropDownRefresh);

```

方法二：

在vue文档中是介绍使用vue.component(tagName,options)来创建全局组件。这种方法是与根实例写在同一个文件中，如果要同时注册多个全局组件，就会是根实例文件过重。

```
1.我们创建全局组件的文件components.js
2.在components.js文件中引入所有要注册的全局组件
3.在main.js根实例中引入components.js

在component.js
import eg from '../components/eg.vue';
export default (Vue)=>{
 Vue.component("Eg",eg);
}

在main.js
import components from './plugins/components.js';
Vue.use(components);
```





