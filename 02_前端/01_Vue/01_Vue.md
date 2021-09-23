# 1、Vue概念

## 1.1、Vue是什么

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与[现代化的工具链](https://cn.vuejs.org/v2/guide/single-file-components.html)以及各种[支持类库](https://github.com/vuejs/awesome-vue#libraries--plugins)结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

## 1.2、Vue安装

官方网站：

1. 开发版本：包含完整的警告和调试模式

   ```html
   <!-- 开发环境版本，包含了有帮助的命令行警告 -->
   <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
   ```

   

2. 生产版本：删除了警告，进行了压缩

   ```xml
   <!-- 生产环境版本，优化了尺寸和速度 -->
   <script src="https://cdn.jsdelivr.net/npm/vue"></script>
   ```

## 1.3、第一个Vue程序

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>Helloworld</title>
</head>
<body>
    <h1>Hello World</h1>
    <hr>
    <div id="app">
        {{message}}
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                message:'hello Vue!'
            }
        })
    </script>
</body>
</html>
```



# 2、内部指令

## 2.1、v-if、v-else、v-show指令

### 2.1.1、v-if、v-else

`v-if`用来判断是否加载html的DOM

比如我们模拟一个用户登录状态，在用户登录后现实用户名称。这里我们在vue的data里定义了isLogin的值，当它为true时，网页就会显示：你好：JSPang，如果为false时，就显示请登录后操作。

```html
<body>
    <h1>v-if 判断是否加载</h1>
    <hr>
    <div id="app">
        <div v-if="isLogin">你好：JSPang</div>  <!-- 【1】、v-if： -->
        <div v-else>请登录后操作</div>          <!-- 【2】、v-else： -->
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
               isLogin:false                   // 【3】、isLogin：如果为false时，v-if指令的div不显示
            }
        })
    </script>
</body>
```

### 2.1.2、v-show

调整css中`display`属性，DOM已经加载，只是CSS控制没有显示出来。

```html
<div v-show="isLogin">你好：JSPang</div>
```

### 2.1.3、v-if 和v-show的区别

`v-if`： 判断是否加载，可以减轻服务器的压力，在需要时加载。

`v-show`：调整css dispaly属性，可以使客户端操作更加流畅。

## 2.2、v-for

`v-for`指令是循环渲染一组data中的数组

`v-for` 指令需要以 `item in items` 形式的特殊语法

- `items` 是源数据数组
- `item`是数组元素迭代的别名。

### 2.2.1、基本用法

```html
<body>
    <h1>v-for指令用法</h1>
    <hr>
    <div id="app">
       <ul>
           <li v-for="item in items">
                {{item}}
           </li>
       </ul>
    </div>
    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                items:[20,23,18,65,32,19,54,56,41]
            }
        })
    </script>
</body>
```

这是一个最基础的循环，先在js里定义了items数组，然后在模板中用v-for循环出来，需要注意的是，你需要那个html标签循环，v-for就写在那个上边。

### 2.2.2、排序

```html
<body>
    <h1>v-for指令用法</h1>
    <hr>
    <div id="app">
       <ul>
           <li v-for="item in sortItems">   <!-- 【1】、换为sortItems -->
                {{item}}
           </li>
       </ul>
    </div>
    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                items:[20,23,18,65,6,19,54,56,41]
            },
            computed:{                // 【2】、监听items数组
                sortItems:function(){
                   return this.items.sort(sortNumber);
                }
            }
        });
        function sortNumber(a,b){      // 【3】、自己编写一个方法sortNumber，然后传给我们的sort函数解决这个Bug
            return a-b;
        }
    </script>
</body>
</html>

```

### 2.2.3、对象循环输出

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <script type="text/javascript" src="../assets/js/vue.js"></script>
    <title>V-for 案例</title>
</head>
<body>
    <h1>v-for指令用法</h1>
    <hr>
    <div id="app">
       <ul>
            <li v-for="(student, index) in students">        <!-- 【1】、输出对象数组 -->
                    {{index}} : {{student.name}} - {{student.age}}
            </li>
       </ul>
    </div>
 
    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                students:[
                    {name:'jspang',age:32},
                    {name:'Panda',age:30},
                    {name:'PanPaN',age:21},
                    {name:'King',age:45}
                    ]
            },
            computed:{  // 【2】、设置监听
                sortStudent:function(){
                    return sortByKey(this.students,'age');
                }
            }
        });
        // 【3】、数组对象方法排序:
        function sortByKey(array,key){
            return array.sort(function(a,b){
                var x=a[key];
                var y=b[key];
                return ((x<y)?-1:((x>y)?1:0));
            });
        }
    </script>
</body>
</html>
```

## 2.4、v-text、v-html

我们已经会在html中输出data中的值了，我们已经用的是`{{xxx}}`,这种情况是有弊端的，就是当我们==网速很慢或者javascript出错时==，会暴露我们的`{{xxx}}`。Vue给我们提供的`v-text`,就是解决这个问题的。

```html
<span>{{ message }}</span> 等同于 <span v-text="message"></span><br/>        
```



如果在javascript中写有html标签，用`v-text`是输出不出来的，这时候我们就需要用`v-html`标签了。

```html
<span v-html="msgHtml"></span>
```

> 【注意】双大括号会将数据解释为纯文本，而非HTML。为了输出真正的HTML，你就需要使用`v-html` 指令。 需要注意的是：在生产环境中动态渲染HTML是非常危险的，因为容易导致`XSS`攻击。所以只能在可信的内容上使用`v-html`，永远不要在用户提交和可操作的网页上使用。

## 2.5、v-on

`v-on` 就是监听事件，可以用v-on指令监听DOM事件来触发一些javascript代码。

### 2.5.1、v-on应用

```html
<body>
<div id="app">
    本场比赛得分： {{count}}<br/>  
    <button v-on:click="jiafen">加分</button>    <!-- 【1】、绑定click事件 -->
</div>

<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            count:1,
            fengshu2: 0
        },
        methods:{ // 【2】、添加click事件方法
            jiafen:function(){
                this.count++;
            }
        }
    })
</script>
</body>

```

### 2.5.2、v-on简化写法

`v-on` 还有一种简单的写法，就是用@代替。

```html
<button @click="jianfen">减分</button>
```

## 2.6、v-model

`v-model`指令，我理解为绑定数据源。就是把数据绑定在特定的表单元素上，可以很容易的实现双向数据绑定。

### 2.6.1、简单的双向数据绑定

在`<input>`中输入值后，`<p>`也跟着显示

```html
<body>
<div id="app">
    <p>原始文本信息：{{message}}</p>
    <h3>文本框</h3>
    <p>v-model1:<input type="text" v-model="message"></p>
</div>
<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            message:'hello Vue!'
        }
    })
</script>
</body>
```

### 2.6.2、修饰符

- `.lazy`：取代 imput 监听 change 事件。
- `.number`：输入字符串转为数字。
- `.trim`：输入去掉首尾空格。

```html
<p>v-model.lazy:<input type="text" v-model.lazy="message"></p>
<p>v-model.number:<input type="text" v-model.number="message"></p>
<p>v-model.trim<input type="text" v-model.trim="message"></p>
```

### 2.6.3、文本域绑定

```html
<textarea cols="30" rows="10" v-model="message"></textarea>
```

### 2.6.4、多选按钮绑定

```html
<body>
<div id="app">
    <p>
        <input type="checkbox" id="JSPang" value="JSPang" v-model="web_Names">
        <label for="JSPang">JSPang</label><br/>
        <input type="checkbox" id="Panda" value="Panda" v-model="web_Names">
        <label for="JSPang">Panda</label><br/>
        <input type="checkbox" id="PanPan" value="PanPan" v-model="web_Names">
        <label for="JSPang">PanPan</label>
    <p>{{web_Names}}</p>
    </p>
</div>

<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            web_Names: []   // 【注意】这里是数组绑定
        }
    })
</script>
</body>
```

### 2.6.5、单选按钮绑定

```html
<body>
<div id="app">
    <input type="radio" id="one" value="男" v-model="sex">
    <label for="one">男</label>
    <input type="radio" id="two" value="女" v-model="sex">
    <label for="one">女</label>
    <p>{{sex}}</p>
</div>
<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            sex: ''
        }
    })
</script>
</body>
```

## 2.7、v-bind

`v-bind`是处理HTML中的标签属性的

```html
<body>
<div id="app">
    <img v-bind:src="imgSrc"  width="200px">   <!-- 动态添加图片 -->
</div>
<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
          imgSrc:'http://baidu.com/wp-content/uploads/2017/02/vue01-2.jpg'
        }
    })
</script>
</body>
```

### 2.7.1、v-bind缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

### 2.7.2、绑定css样式

在工作中我们经常使用v-bind来绑定css样式：

在绑定CSS样式是，绑定的值必须在vue中的data属性中进行声明。

**1）、直接绑定class样式**

```html
<div :class="className">1、绑定classA</div>
```

**2）、绑定classA并进行判断，在isOK为true时显示样式，在isOk为false时不显示样式。**

```html
<div :class="{classA:isOk}">2、绑定class中的判断</div>
```

**3）、绑定class中的数组**

```html
<div :class="[classA,classB]">3、绑定class中的数组</div>
```

**4）、绑定class中使用三元表达式判断**

```html
<div :class="isOk?classA:classB">4、绑定class中的三元表达式判断</div>
```

**5）、用对象绑定style**

```html
<body>
<div id="app">
    <div :style="styleObject">6、用对象绑定style样式</div>
</div>
<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            styleObject:{
                fontSize:'24px',
                color:'green'
            }
        }
    })
</script>
</body>
```

## 2.8、其他内部指令

### 2.8.1、v-pre

在模板中跳过vue的编译，直接输出原始值。就是在标签中加入v-pre就不会输出vue中的data值了。

```html
<div v-pre>{{message}}</div>
```

这时并不会输出我们的message值，而是直接在网页中显示{{message}}

### 2.8.2、v-cloak

当网速较慢，Vue.js 文件还没有加载完时，在页面会显示{{message}}的字样，直到Vue创建实例、编译模版时，DOM才会被替换，所以这个过程屏幕是有闪动的，需要配合CSS可以解决这个问题。

```

```

```html
<body>
<div id="app">
    <div v-cloak>{{ message }}</div>
</div>
<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            message: '上岸了'
        }
    })
</script>
<style type="text/css">
	[v-cloak] {
		display: none
	}
</style>
</body>
```

### 2.8.3、v-once

在第一次DOM时进行渲染，渲染完成后视为静态内容，跳出以后的渲染过程。

```html
<div v-once>第一次绑定的值：{{message}}</div>       <!-- 第一次为动态输入的值，之后修改的值不在变化 -->
<div><input type="text" v-model="message"></div>
```

# 3、全局API

## 3.1、什么是全局API

全局API并不在构造器里，而是先声明全局变量或者直接在Vue上定义一些新功能，Vue内置了一些全局API

## 3.2、Vue.directive

Vue.directive 自定义指令，类似v-model、v-show、v-html一样的指令

```html
<body>
<h1>Vue.directive 自定义指令</h1>
<hr/>
<div id="app">
    <div v-jspang="color">{{num}}</div>
</div>

<script>
    Vue.directive("jspang", function (el, binding, v) {
        el.style = "color: " + binding.value;
    });
    var app = new Vue({
        el: '#app',
        data: {
            color: 'green'
        }
    });
</script>

```

> 【说明】Vue.directive中钩子函数的三个参数
>
> ①、`el`：指令所绑定的元素，可以用来直接操作DOM。
>
> ②、`binding`： 一个对象，包含指令的很多信息。
>
> ③、`vnode`：Vue编译生成的虚拟节点



### 3.2.1、生命周期

自定义指令有五个生命周期（也叫钩子函数），分别是 `bind,inserted,update,componentUpdated,unbind`

1. `bind`：只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个绑定时执行一次的初始化动作。
2. `inserted`：被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于document中）。
3. `update`：被绑定于元素所在的模板更新时调用，而无论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。
4. `componentUpdated`：被绑定元素所在模板完成一次更新周期时调用。
5. `unbind`：只调用一次，指令与元素解绑时调用。

```html
<script>
    Vue.directive("jspang",{
        bind:function(){//被绑定
            console.log('1 - bind');
            el.style = 'color: ' + binding.value;
        },
        inserted:function(){//绑定到节点
            console.log('2 - inserted');
        },
        update:function(){//组件更新
            console.log('3 - update');
        },
        componentUpdated:function(){//组件更新完成
            console.log('4 - componentUpdated');
        },
        unbind:function(){//解绑
            console.log('5 - bind');
        }
    });
</script>
```

## 3.3、Vue.extend

```html
<body>
    <authoor></authoor>
    <div id="authoor"></div>

    <script>
        var authorExtend = Vue.extend({
            template: '<p><a :href="authorURL">{{authorName}}</a></p>',
            data: function () {
                return {
                    authorName: 'JSPang',
                    authorURL: 'http://jspang.com'
                }
            }
        });

        // 这时html中的标签还是不起作用的，因为扩展实例构造器是需要挂载的，我们再进行一次挂载。
        new authorExtend().$mount('authoor');   // 挂载到<authoor>标签上
        new authorExtend().$mount('#authoor');  // 挂载到普通标签上，这里是<div>。建议使用ID的形式
    </script>
</body>
```

## 3.4、Vue.set

`Vue.set` 的作用就是在构造器外部操作构造器内部的数据、属性或者方法

```html
<body>
<h1>Vue.set 全局设置</h1>
<hr/>
<div id="app">
    {{this.count}}
</div>
<p><button onclick="add()">add</button></p>

<script>
    // 【3】、在外部改变数据
    function add(){
        // 方式1, 用Vue.set改变
        Vue.set(outDate, 'count', 2);
        // 方式2, 用Vue对象的方法添加
        app.count ++;
        // 方式3, 直接操作外部数据
        outDate.count ++;
    }

    var outDate = { // 【1】、定义外部数据（Vue构造器之外的数据）
        count : 1,
        goods : 'car'
    }

    var app = new Vue({
        el: '#app',
        data: outDate // 【2】、引用外部数据
    });
</script>
</body>
```

为什么要有Vue.set：由于Javascript的限制，Vue不能自动检测以下变动的数组。

- 当你利用索引直接设置一个项时，vue不会为我们自动更新。
- 当你修改数组的长度时，vue不会为我们自动更新。

## 3.5、Vue的生命周期（钩子函数）

Vue一共有10个生命周期函数，我们可以利用这些函数在vue的每个阶段都进行操作数据或者改变内容。

![Vue 实例生命周期](images\01_Vue\lifecycle.png)

```html
<body>
    <div id="app">
        {{count}}
        <p><button @click="add">add</button></p>
    </div>
    <button onclick="app.$destroy();">销毁</button>

    <script>
        var app = new Vue({
            el: '#app',
            data: {
                count: 1
            },
            methods:{
                add: function(){
                    this.count ++;
                }
            },
            beforeCreate:function(){
                console.log('1-beforeCreate 初始化之前');
            },
            created:function(){
                console.log('2-created 创建完成');
            },
            beforeMount:function(){
                console.log('3-beforeMount 挂载之前');
            },
            mounted:function(){
                console.log('4-mounted 被创建(挂载之后)');
            },
            beforeUpdate:function(){
                console.log('5-beforeUpdate 数据更新前');
            },
            updated:function(){
                console.log('6-updated 数据更新后');
            },
            activated:function(){
                console.log('7-activated');
            },
            deactivated:function(){
                console.log('8-deactivated');
            },
            beforeDestroy:function(){
                console.log('9-beforeDestroy 销毁之前');
            },
            destroyed:function(){
                console.log('10-destroyed 销毁之后')
            }
        });
    </script>
</body>
```

## 3.6、Template制作模板

### 3.6.1、直接写在选项里的模板

直接在构造器里的template选项后边编写。这种写法比较直观，但是如果模板html代码太多，不建议这么写。

```html
<body>
<div id="app">    <!-- 通过id将 div渲染为template中定义的标签 -->
    {{message}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: 'hello would'
        },
        template:` 
            <h2 style="color: red">我是选项模板</h2>
        ` // 这里需要注意的是模板的标识不是单引号和双引号，而是，就是Tab上面的键。
    });
</script>
</body>
```

### 3.6.2、卸载template标签里的模板

这种写法更像是在写HTML代码，就算不会写Vue的人，也可以制作页面。

```html
<body>
    <div id="app">
        <template id="demo2">
            <h2 style="color:red">我是template标签模板</h2>
        </template>
    </div>
    <script>
        var app = new Vue({
            el:'#app',
            data:{
                message:'hello Vue!'
            },
            template:'#demo2'
        });
    </script>
</body>
```

### 3.6.3、写在script标签里的模板

这种写模板的方法，可以让模板文件从外部引入。

```html
<body>
    <div id="app"></div>
    <script type="x-template" id="demo3">   <!--模板在前，否则在vue构造器无法加载-->
        <h2 style="color:red">我是script标签模板</h2>
    </script>
    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                message:'hello Vue!'
            },
            template:'#demo3'
        })
    </script>
</body>
```

## 3.7、Component

用于制作自定义的标签

### 3.7.1、全局化注册组件

全局化就是在构造器的外部用Vue.component来注册，我们注册现在就注册一个的组件来体验一下。

```vue
<body>
    <h1>hello would</h1>
    <hr/>
    <div id="app">
        <jspang></jspang>
    </div>
    <div id="ppa">
        <jspang></jspang>
    </div>

    <script>
        Vue.component('jspang', {
            template: `<div style="color:red;">我是全局的jspan组件</div>`
        });

        var app = new Vue({
            el: '#app'
        });
    </script>
</body>
```

### 3.7.2、局部注册组件局部

局部注册组件局部注册组件和全局注册组件是向对应的，局部注册的组件只能在组件注册的作用域里进行使用，其他作用域使用无效。

```vue
<body>
    <div id="app">
      <panda></panda>
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            components:{ // 构造器里的components 是加s的，而全局注册是不加s的
                "panda":{
                    template:`<div style="color:red;">局部注册的panda标签</div>`
                }
            }
        })
    </script>
</body>
```

### 3.7.3、组件和指令的区别

组件注册的是一个标签，而指令注册的是已有标签里的一个属性。在实际开发中我们还是用组件比较多，指令用的比较少。

### 3.7.4、props属性设置

`props`选项就是设置和获取标签上的属性值的

#### ①、定义属性并获取属性值

```vue
<body>
    <div id="app">
      <panda here="China"></panda>    <!-- 最后输出的结果是红色字体的【Panda from China】 -->
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            components:{
                "panda":{
                    template:`<div style="color:red;">Panda from {{ here }}.</div>`,
                    props:['here']  // 将here属性值里的China传递了给组件
                }
            }
        })
    </script>
</body>
```

> 【注意】在写属性时经常会加入’-‘来进行分词，比如：，那这时我们在props里如果写成props:[‘form-here’]是错误的，我们必须用==小驼峰式==写法props:[‘formHere’]。

#### ②、在构造器里向组件传值

把构造器中data的值传递给组件，我们只要进行绑定就可以了

```vue
<body>
    <div id="app">
        <panda v-bind:here="message"></panda>   <!-- 绑定 -->
    </div>

    <script type="text/javascript">
        var app=new Vue({
            el:'#app',
            data:{
                message:'SiChuan' 
            },
            components:{
                "panda":{
                    template:`<div style="color:red;">Panda from {{ here }}.</div>`,
                    props:['here']
                }
            }
        })
    </script>
</body>
```

### 3.7.5、父子组件关系

在实际开发中我们经常会遇到在一个自定义组件中要使用其他自定义组件，这就需要一个父子组件关系。

```vue
<body>
    <div id="app">
        <panda></panda>    <!-- 【4】、使用组件 -->    
    </div>

    <script>
        // 【1】、定义组件1
        var city = {
            template: `<div style="color: green;">siChuan of China.</div>`
        }
        // 【2】、定义组件2
        var pandaComponent = {
            template: `<div>
                            <p>Panda from China!</p>
                            <city></city>
                        </div> `,   // 【2.2】、在组件2中 嵌套 组件1
            components: { // 【2.1】、声明组件1，将其引入到组件2中
                'city': city
            }
        }

        var app = new Vue({
            el: '#app',
            components: { // 【3】、声明组件2，将其引入到vue中
                'panda': pandaComponent
            }
        });

    </script>
</body>
```

### 2.7.6、Component标签

Component标签是Vue框架自定义的标签，它的用途就是可以动态绑定我们的组件，根据数据的不同更换不同的组件。

```vue
<body>
<div id="app">
    <!-- 【3】、我们在html里插入component标签，并绑定who数据，根据who的值不同，调用不同的组件。-->
    <component v-bind:is="who"></component>
    <button @click="changeComponent">changeComponent</button>
</div>

<script>
    // 【1】、我们先在构造器外部定义三个不同的组件，分别是componentA,componentB和componentC.
    var componentA = {
        template: `<div style="color: red">I'm componentA.</div>`
    }
    var componentB = {
        template: `<div style="color: green">I'm componentB.</div>`
    }
    var componentC = {
        template: `<div style="color: pink">I'm componentC.</div>`
    }

    var app = new Vue({
        el: '#app',
        data: {
            who: 'componentA'
        },
        // 【2】、我们在构造器的components选项里加入这三个组件。
        components:{
            'componentA': componentA,
            'componentB': componentB,
            'componentC': componentC
        },
        methods:{
            // 【4】、这就是我们的组件标签的基本用法。我们提高以下，给页面加个按钮，每点以下更换一个组件。
            changeComponent: function () {
                if(this.who === 'componentA'){
                    this.who = 'componentB';
                } else if(this.who === 'componentB'){
                    this.who = 'componentC';
                } else {
                    this.who = 'componentA';
                }
            }
        }
    });
</script>
</body>
```

# 4、选项

## 4.1、propsData

propsData Option用在全局扩展时进行传递数据

```html
<body>
    <header></header>
    <script>
        var header_a = Vue.extend({
            template: `<p>{{message}}-{{a}}</p>`,
            data: function(){
                return {
                    message: 'Hello, I am header!'
                }
            },
            props: ['a']
        });
        var app = new Vue({
            el: '#app',
            data: {
                message: 'hello would'
            }
        });

        // 将header_a挂载到header标签组件中
        new header_a({propsData:{a:'测试'}}).$mount('header');
    </script>
</body>
```

总结：propsData在实际开发中我们使用的并不多，我们在后边会学到==Vuex==的应用，他的作用就是在单页应用中保持状态和数据的。





































# 5、实例和内置对象