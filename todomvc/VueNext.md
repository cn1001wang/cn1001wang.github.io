# 结果演示

[Vue3.0 TodoMVC](http://jsrun.net/wyvKp/edit)

# 导论

[composition-api](https://composition-api.vuejs.org/zh/)估计离发版不远了，已经出中文了（之前看文档时候还只有英文，但我一直拖延症没出文章，搞得中文版都已经出来了）
![](https://user-gold-cdn.xitu.io/2020/5/24/17245e182f38ff63?w=919&h=306&f=jpeg&s=27845)

## 别担心，Vue3.0也是很简单的，加入进来吧

别喊学不动了，拉我起来，我还能学（😄）

在本文中，我将用最easy的学习方式帮你过一遍Vue3.0的全新语法。
将只会涉及到**声明式渲染部分**,学完你就可以吹吹牛逼，搞搞小项目了
![](https://user-gold-cdn.xitu.io/2020/5/24/17245f5f9792b799?w=1913&h=762&f=png&s=697656)

# step 0： 我们的尤大不辞辛劳，帮我们做兼容Vue2.0写法的处理

## 新手只想看3.0的，可以跳过本段

你不会2.0？哦，太棒了，从头学，你可以不受**Vue2.0**的干扰.下面只是展示一下回退到2.0的写法会怎样。  
可能由于当时推出Composition-API时候，社区反映太强烈，**3.0**也提供了类似**2.0**写法。我在这边也只是做一个简单的展示，关公面前耍大刀，各位大佬见笑多包涵。

```html
<html>
<body>
    <div id="app">
        <input type="text" v-model="newTodo" />
        <p>{{newTodo}}</p>
        <button @click="say">大声说爱你</button>
    </div>
    <script src="https://unpkg.com/vue@3.0.0-beta.14/dist/vue.global.js"></script>
    <script>
        var app=Vue.createApp({
            data:{
                newTodo:""
            },
            methods:{
                say(){
                    console.log(this.newTodo)
                }
            }
        })
        var p=app.mount("#app")
    </script>
</body>
</html>
```

`<script src="https://unpkg.com/vue@3.0.0-beta.14/dist/vue.global.js"></script>`这个文件会暴露出来一个全局的Vue变量，可以让我们通过 `Vue.crateApp({}).mount("一个Dom或选择器")`创建一个Vue实例后挂载到一个Dom上。

现在数据和 DOM 已经被建立了关联，所有东西都是响应式的。

`methods`这个属性提供的say方法，目前是能够通过this访问到data中的属性，若有使用有误，我会更进修改。

这样你如果你只是需要一个快速生成form表单的JS库，他能够很轻松的完成了

# step 1: 走进Vue 3.0的世界

```html
<html>
<body>
    <div id="app">
        <input type="text" v-model="newTodo" />
        <p>ref:{{newTodo}}</p>
        <button @click="say">大声说爱你</button>
	      <p>reactive:{{obj.count}}</p>
        <button @click="Add">Add</button>
    </div>
    <!-- <script src="https://unpkg.com/vue@3.0.0-beta.14/dist/vue.global.js"></script> -->
    <script src="./vue-next@3.0.0-beta.14.js"></script>
    <script>
        var app=Vue.createApp({
            setup(){
              	const obj = reactive({ count: 0 });
                var newTodo=Vue.ref("");
                function say(){
                    console.log(newTodo.value);
                }
              	function Add(){
                    obj.count++;
                }
                return {
                    newTodo,//相当于{ newTodo:newTodo,say:say }
                    say,
                  	obj,
	                  Add
                }
            }
        })
        var p=app.mount("#app")
    </script>
</body>
</html>
```

你可以将本例复制下来，双击运行，OK，你会**Vue3.0**了，你可以退出去了(😄)。
**讲正经的**。在vue3中增加一个setup的方法用于初始化整个项目，可以简单的理解为在setup中定义好变量和方法，将step0中的data与methods整合到setup中。

### [setup](https://vue-composition-api-rfc.netlify.app/zh/api.html#setup)

 函数是一个新的组件选项。作为在组件内使用 Composition API 的入口点。

### [reactive](https://vue-composition-api-rfc.netlify.app/zh/api.html#reactive)

接收一个普通对象然后返回该普通对象的响应式代理。等同于 2.x 的 `Vue.observable()`。

### [ref](https://vue-composition-api-rfc.netlify.app/zh/api.html#ref)

接受一个参数值并返回一个响应式且可改变的 ref 对象。ref 对象拥有一个指向内部值的单一属性 `.value`。

# 补充

- [删除filters的支持](https://juejin.im/post/5e12a2e95188253ab321aa8d#heading-0)
