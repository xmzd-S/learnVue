<a name="jqyb3"></a>
# 第一章 引言
<a name="Oikhc"></a>
## 1.1 hello,world
新建文件夹，将文件夹拖到VSCode软件图标上即可打开该文件夹。<br />输入一个!+tab 可以快速生成html模板<br />引入vue.js，有两种方式，一种是引入CDN  
```java
<script src="https://unpkg.com/vue@next"></script>
```
一种是直接引入vue的文件（打开CDN的网址然后另存为）
```java
<script src="vue.global.js"></script>
```

创建对象Vue.createApp({对象}).mount("被挂载的dom对象")<br />templet属性：会在被挂载dom对象下创建内容
```html

  <script>
    Vue.createApp({
      
      template:"<div>hello</div>"
    }).mount("#root");
  </script>
</html>
```
在webstorm中点击vue的链接，可以下载，然后新建一个lib文件夹，拷贝进去。然后src修改如下
```javascript
<script src="lib/vue.js"></script>
```


<a name="CU3hJ"></a>
## 1.2 取值语法和data方法属性
取值语法：{ { } }<br />data方法属性：返回的是一个对象。template属性的内容会从data方法返回的对象中根据属性名取值
```html

<script>
    Vue.createApp({
        data(){
            return {
                content:'hello'
            }
        },
        template:"<div>{{content}}</div>"
    }).mount("#root");
</script>
</html>
```

<a name="TgFlD"></a>
## 1.3 mounted方法属性
mounted方法:在元素加载完后执行的方法<br />setInterval方法，第一个参数是方法，第二个参数是间隔的毫秒。
```html

<script>
    Vue.createApp({
        data(){
            return {
                content:0
            }
        },
        template:"<div>{{content}}</div>",
        mounted(){
            setInterval(
                ()=>{
                    this.content +=1
                }//箭头函数，相当于java中的匿名对象
              ,1000
            )
        }
    }).mount("#root");
</script>
</html>
```

<a name="XJOIf"></a>
## 1.4 v-命令
下面的这些都是出现在模板中的属性中的<br />注意，多行字符串是使用`  `<br />v-on：click属性，绑定的方法在methods中<br />v-if属性：注意，标签内的属性格式为属性名="属性值". template可以直接输入data返回的对象的属性名，但是methods和mounted中却不可以，需要前面加个this

v-if属性=属性值。属性值是data返回的对象的属性名，可以不用加this。_v-if 的意思是该标签的存在与否取决于属性值_<br />v-on:click=属性值,属性值是methods对象中的某个方法名_v-on:click 是vue中模板绑定点击事件的语法。使用的事件函数在methods中_

```html
<script>
    Vue.createApp({
        data(){
            return{
                show: true
            }
        },
        methods:{
            handleClick(){
                this.show=!this.show;
            }
        },
        template:`<span v-if="show">hello world</span>
                    <button v-on:click="handleClick" >显示/隐藏</button>
        `

    }).mount("#root");
</script>
</html>
```
v-for:_v-for的意思是该标签循环输出，属性值语法为 变量名1 of 变量名2 ，innerhtml为{{变量名1}}_
```javascript
<script>
    Vue.createApp(
        {
            data(){
                return{
                    list:[1,2,3,4,5]
                }
            },
            template:`
                <ul> 
                    <li v-for="item of list">{{item}}</li>
                </ul>`
        }
    ).mount("#root");
</script>
</html>
```
v-model双向绑定_v-model的意思是标签内的值与v-model的属性值进行双向绑定，该属性值在data区域中_

```javascript
<script>
    Vue.createApp(
        {
            data(){
                return{
                    list:[]
                }
            },
            methods:{
                handleClick(){
                    this.list.push(this.inputValue);
                    this.inputValue='';
                }
            },
            template:`
                <input v-model="inputValue"/>
                <button v-on:click=handleClick>提交 </button>
                <ul>
                    <li v-for="item of list">{{item}}</li>
                </ul>`
        }


    ).mount("#root");
</script>
</html>
```
v-bind:标签的属性绑定某一个变量的时候使用v-bind冒号后面是原先标签的属性_v-bind:属性 的意思是标签的属性与data区域中的变量进行绑定_


```javascript
   Vue.createApp(
        {
            data(){
                return{
                    list:[]
                }
            },
            methods:{
                handleClick(){
                    this.list.push(this.inputValue);
                    this.inputValue='';
                }
            },
            template:`
                <input v-model="inputValue"/>
                <button v-on:click=handleClick 
                        v-bind:title="inputValue"
                
                >提交 </button>
                <ul>
                    <li v-for="item of list">{{item}}</li>
                </ul>`
        }


    ).mount("#root");
```
<a name="VSl8P"></a>
## 1.5 组件的演示
组件就是页面的一部分，最小的组件其实就是一个dom标签。<br />component函数来注册一个组件，component函数的第一个参数是一个字符串，也是组件名，也是标签名。<br />component函数的第二个参数是一个对象，有很多属性，比如template属性,作为组件的子标签的内容。<br />下面的例子中，v-bind:content属性为该组件注册了一个名为content的属性，该属性可以被组件中通过props属性得到。<br />最后再挂载实例

props属性，以及组件间传递属性<br />_//先创建实例，再定义组件，最后再挂载<br />//v-bind:属性名=属性值，为该标签绑定了一个变量，这个变量也可以是v-语句中的变量<br />//组建的props属性可以获取v-bind的属性_
```javascript
<script>
    const app = Vue.createApp(
        {
            data(){
                return{
                    list:[]
                }
            },
            methods:{
                handleClick(){
                    this.list.push(this.inputValue);
                    this.inputValue='';
                }
            },
            template:`
                <input v-model="inputValue"/>
                <button v-on:click=handleClick 
                
                >提交 </button>
                <ul>
                    <todoItem v-for="item of list"  
                              v-bind:content="item"/>
                </ul>`
        }


    );
    app.component('todoItem',{
        props:['content'],
        template:'<li >{{content}} </li>'
    });
    app.mount("#root")
</script>
</html>
```
<a name="gan69"></a>
# 第二章 深入理解引言
<a name="oSfZl"></a>
## 2.1 mvvm模型
createApp代表创建一个Vue应用<br />传入的对象表示这个应用最外层的组件，也叫根组件<br />当vue应用挂载到某一个dom对象后返回的对象vm，就是根组件<br />vue是面向数据编程，借鉴了mvvm，m-->数据，v--->视图，vm-->viewAndModel视图数据连接层，比如下面，data中放的是m，template放的是视图，它们放在一个vue参数中，这个参数是vue的组件，自动组成了视图数据连接层。数据是我们写的，模板也是我们写的，但是vm层却是组件帮我们做的。
```html
<script>
    const app = Vue.createApp(
        {
            data(){
                return {content:1 }
            },
            template:`<div>{{content}} </div>`
        }

    );
    const b = app.mount("#root");
    console.log(b)
</script>
</html>
```

<a name="PJbxy"></a>
## 2.2 获取vue实例的属性
在vue中，组件的属性可以用对象.$属性名得到.比如在控制台输入b.$data，可以输出data返回的对象<br />如果我们输入b.$data.content='bye' ，这样就修改了数据层的数据，组件会帮我们自动渲染，使得视图层也发生相对应的变化



<a name="Tgy6V"></a>
## 2.3 生命周期函数
生命周期函数：在某一时刻会自动执行的函数。事件绑定函数并非生命周期函数，生命周期函数比如mounted函数


vue在creatApp以及mount之后会首先初始化events和lifecycle（初始化事件函数比如v-on等事件函数和生命周期函数比如mounted函数，注意，这里只是初始化并不执行），<br />然后调用第一个生命周期函数beforeCreate函数，之后init injections和reactivity（初始化依赖注入和双向绑定），<br />![1656304283(1).png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656304490733-57ea1f89-d981-45ad-a4c0-fe115a537894.png#averageHue=%23d2cdba&clientId=u90c69363-c883-4&from=paste&height=358&id=ub996dbd3&originHeight=358&originWidth=731&originalType=binary&ratio=1&rotation=0&showTitle=false&size=89844&status=done&style=none&taskId=ud35d7bea-3603-4298-bed6-e53df948796&title=&width=731)

之后执行created函数。<br />![8c5e575fca14e166d1e9e48dc32f9fc.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656305037601-db3079ed-57d9-42e4-975d-ebeea22acef9.jpeg#averageHue=%23bdb49d&clientId=u90c69363-c883-4&from=paste&height=1080&id=ub6978af6&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=248531&status=done&style=none&taskId=u34c57097-2608-4e7f-848a-99a7e158de2&title=&width=1728)

然后判断是否存在"template"这个选项，如果有这个选项，那么vue会将其变为一个函数render，之后执行beforeMount函数.之后函数和数据层结合形成一个新的dom对象，然后用新的dom替换页面上id=root的这个标签的内容，这个时候组件已经挂载到dom上了。之后执行mounted函数。<br />综上：

- beforeCreate函数在实例生成之前会自动执行的函数
- created函数在实例生成之后会自动执行的函数
- beforeMount函数在组件内容被渲染到页面之前立即自动执行的函数
- mounted函数在组件内容被渲染到页面之后立即自动执行的函数
- 当mounted函数执行之后，页面上就显示了数据了。

最后如果数据发生变化，变化之前会执行beforeUpdate，变化之后会执行updated（在控制台vm.$data来改变数据试试）<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656305451731-2f5b9aa9-5007-4302-929f-c0c943dca953.png#averageHue=%23cfc4b1&clientId=u90c69363-c883-4&from=paste&height=280&id=u646819f1&originHeight=280&originWidth=435&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75159&status=done&style=none&taskId=u9870d938-7f69-4b7c-a654-1ea12e96b74&title=&width=435)


![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656305651069-2108e481-c556-4d34-b8ed-e1572fa43de7.png#averageHue=%23fdfdfc&clientId=u90c69363-c883-4&from=paste&height=208&id=u6c83e5d4&originHeight=208&originWidth=376&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8729&status=done&style=none&taskId=u12c52734-3282-4d34-a56f-7f8e0a62883&title=&width=376)

```html

<script>
   // 生命周期函数
    let app = Vue.createApp({
        data(){
            return{
                message:'hello,world'
            }
        },
        //在实例生成之前执行的函数
        beforeCreate(){
            console.log("beforeCreate生命周期函数 1")
        },
        //在实例生成之后执行的函数
        created(){
            console.log("created生命周期函数 2")
        },
        //在组件内容被渲染到页面之前执行的函数
        beforeMount(){

            console.log("beforeMount生命周期函数 3")
            //获取不到内容
            console.log(document.getElementById('root').innerHTML);
        },
        //在组件内容渲染到页面函数之后执行的函数.
        mounted: function () {

            console.log("mounted生命周期函数 4");
            //是可以获取到内容的
            console.log(document.getElementById('root').innerHTML);

        },

        template:`<div>{{message}}</div>`,
        beforeUpdate(){
            console.log("beforeUpdate生命周期函数 5");
        },
        updated(){
            console.log("updated生命周期函数 6");
        }
    });
    let vm = app.mount('#root');
</script>
</html>
```




另外，还有两个生命周期函数，这两个周期函数是在vue应用（而不是组件）从挂载的dom上卸载下来的过程中调用的。<br />vue调用unmount方法可以使得vue应用从挂载的dom上卸载下来<br />当Vue应用失效时，自动执行beforeUnmount函数<br />当Vue应用失效且dom完全销毁之后会调用unmounted函数

```javascript
beforeUnmount(){
  console.log(document.getElementById("root").innerHTML,"beforeUnmount")
},
  unmounted(){
    console.log(document.getElementById("root").innerHTML,"unmounted")
            }
```




**补充：如果没有template属性，那么会将被挂载的dom对象的innerhtml作为template。所以template的语句可以写在被挂载的dom对象下，插值语法也可以使用。**


<a name="k2dCS"></a>
## 2.4 模板常用语法

1. 插值表达式取数据区域中的数据
2. v-html的使用：可以将数据中的标签进行转义
3. v-bind的使用，给模板中的标签属性绑定数据区域中的值
4. 插值表达式中输入表达式
5. v-once  表示该插值表达式中只有第一次渲染时会与数据区域保持一致，而以后如果数据区域发生变化，该插值表达式不会随着数据区域的变化而变化
6. v-if: 该标签展示或者不展示是由v-if的值决定的，这个值是数据区域中的
7. v-on:click 属性值为方法名，方法在methods中
8. 缩写：

v-on:click  @click<br />v-bind:属性名     :属性名

9. 动态参数

动态属性    :[数据区域中的变量名]=数据区域中的变量名 ，属性绑定时属性名也可以是非固定的<br />动态事件    @[数据区域中的变量名]=methods中的方法名

10. 修饰符:@click.prevent=方法名，阻止这个标签点击时的默认行为
```javascript
<script>
    const app = Vue.createApp(
      {
        data(){
          return{
            messageOne:'hello,world',
            messageTwo:"<div>这是一个div</div>",
            messageThree:"v-bind的使用",
            messageFour:"v-once",
            messageFive:false,
            messageSix:"test",
            messageSeven:"testValue",
            clickOne:"click"
            
          }
        },
        methods:{
          handleClickOne(){
            this.messageFive=!this.messageFive;
          },
          handleClickTwo(){
            alert("handleClickTwo")
          }
          
        },
        //模板语法一：插值语法
        template:`<div>{{messageOne}}</div>
        <!--模板语法二：转义            -->
        <div v-html="messageTwo"></div>
        <!--模板语法三：v-bind-->
        <div v-bind:title="messageThree">{{ messageThree }}</div>
        <!--模板语法四：直接输入表达式-->
        <div>{{12+22}}</div>
        <!--模板语法五：v-once的使用            -->
        <div v-once>{{messageFour}}</div>
        <!--模板语法六：v-if的使用-->
        <div v-if="messageFive">测试一下v-if</div>
        <!--模板语法七：v-on:click的使用            -->
        <div v-on:click="handleClickOne">点击显示/隐藏上一个v-if</div>
        <!--模板语法八：命令的简写            -->
        <div @click="handleClickOne">点击显示/隐藏上一个v-if</div>
        <div :title="messageThree">{{ messageThree }}</div>
        <!--模板语法九：动态属性-->
        <div :[messageSix]="messageSeven">测试动态属性</div>
        <!--模板语法十：动态事件，这个在elements中无法查看绑定的事件            -->
        <div @[clickOne]="handleClickTwo">测试动态事件</div>
        <!--模板语法十一：事件属性修饰符.prevent：阻止事件的默认行为            -->
        <a href="https://www.baidu.com" @click.prevent>百度</a>
        
        `,
      }
    );
let vm = app.mount("#root");
</script>
</html>
```




<a name="aLKsa"></a>
## 2.5 数据函数对象
也就是data函数对象，数据在返回的对象中。数据可以是基本数据类型也可以是对象
<a name="ympDA"></a>
## 2.6 方法区对象

1. 方法区的this指向的是根组件
2. 方法区中的方法也可以在插值表达式中使用
```javascript
   Vue.createApp(
        {
            data(){
                return{
                    message:1
                }
            },
            //方法区中的this指向的根组件
            //插值语法中可以直接使用方法区中的方法
            methods:{
                handleClickOne(){
                    console.log(this)
                },
                methodOne(){
                    setInterval(
                        ()=>{
                            this.message += 1;
                        },1000
                    )
                }
            },
            template:`<div @click="handleClickOne" >点击事件1</div>
            <div>{{message}}</div>
            <div>{{methodOne()}}</div>`
        }
    ).mount("#root")
```

<a name="LdaNU"></a>
## 2.7 计算属性对象
computed:{方法名(){}},可以对数据区的数据进行计算，且`对数据区的数据进行双向绑定`<br />计算属性和方法区的区别：

1. 计算属性可以在插值表达式中直接写计算属性名即可不用写括号，而方法区中的方法需要写括
2. 计算属性是当计算属性所依赖的内容发生变更时，才会重新执行计算

方法区是只要页面重写渲染，就会重新执行计算
```javascript
   Vue.createApp(
        {
            data(){
                return{
                    price:1,
                    count:5
                }
            },
            computed:{
                total(){
                    return this.price*this.count
                }
            },
            template:`{{total}}`

        }
    ).mount("#root")
```
<a name="ttJHP"></a>
## 2.8 侦听器对象

watch对象中函数名为数据区中的变量名，当数据区中的变量发生改变时，可以异步的调用该侦听器对象中定义的函数。且函数对象的参数第一个是现在的值，第二个参数是改变前的值



computed和methods都能实现的一个功能，建议使用computed，因为缓存<br />computed和watcher都能实现的功能，建议使用computed，因为更加简洁
```javascript
   Vue.createApp(
        {
            data(){
                return{
                    message:"hello"
                }
            },
            watch:{
                message(newOne,oldOne){
                    console.log(newOne,oldOne)
                }
            },
            methods:{
                methodOne(){
                    this.message="world"
                }
            },
            template:`<button @click="methodOne">{{message}}</button>`
        }
    ).mount("#root")
```

<a name="b9g60"></a>
## 2.9 样式绑定

1. 直接在模板中的标签中指定class 或者id属性，然后css在外部处理
2. 利用:bind命令将class属性与数据区域中的数据进行绑定，然后变化处理
3. v-bind:对象
4. v-bind:数组


![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656333189249-2daa8db5-c6dd-4620-a030-2423e07a6fd4.png#averageHue=%23fcfbfb&clientId=u3907d8ce-d62f-4&from=paste&height=123&id=u6e4a05e9&originHeight=123&originWidth=385&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21234&status=done&style=none&taskId=ucfba9b83-07b2-4947-be2a-cb677d00529&title=&width=385)

5. v-bind:$attrs.class 父组件的类名

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656379180993-c8f0ac5f-32a7-459b-a468-7d8555e503ee.png#averageHue=%23f9f7f7&clientId=u5c458e9f-b0f0-4&from=paste&height=294&id=u5f464746&originHeight=294&originWidth=319&originalType=binary&ratio=1&rotation=0&showTitle=false&size=47096&status=done&style=none&taskId=uc5484c65-f3f2-4ad2-b61f-92d774c57c6&title=&width=319)

6. 行内样式：直接在模板中style即可
7. style对象（用v-bind)

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656379313789-2939db8f-6418-44cf-a240-aef1b0cf7b4d.png#averageHue=%23fdf8f8&clientId=u5c458e9f-b0f0-4&from=paste&height=223&id=u411e6274&originHeight=223&originWidth=408&originalType=binary&ratio=1&rotation=0&showTitle=false&size=37094&status=done&style=none&taskId=u04def3dc-f639-4575-a2f1-97a35f15fd5&title=&width=408)


<a name="KQ3EI"></a>
## 2.10 条件渲染

1. v-if:通过控制dom存在与否来控制标签展示与否·
2. v-show：通过display属性为none或者非none来控制标签展示与否

额外：v-else. 如果v-if后加了数据区域中的数据，那么v-else就不用了。if和else一定要在一起。<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656380151166-94df4470-d57a-4202-85cc-ec87b0371978.png#averageHue=%23fcfafa&clientId=u5c458e9f-b0f0-4&from=paste&height=233&id=uae7b3f50&originHeight=233&originWidth=332&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28823&status=done&style=none&taskId=ue2f62a78-0a2e-460b-b9c7-357ecbf9654&title=&width=332)

额外：v-else-if：下面的意思是 conditionOne是true时展示if，<br />若conditionOne是false且conditionTwo是true时展示else if<br />若conditionOne是false且conditionTwo是false时 展示else<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656380247801-1ff81bc1-024a-43b5-8a99-bd4a94441f2a.png#averageHue=%23fbf9f8&clientId=u5c458e9f-b0f0-4&from=paste&height=199&id=ub32b2cbb&originHeight=199&originWidth=301&originalType=binary&ratio=1&rotation=0&showTitle=false&size=31138&status=done&style=none&taskId=u8d6a6e7c-0ce8-4ed3-86aa-3614221f297&title=&width=301)


<a name="n3tGy"></a>
## 2.11 列表循环渲染


1. v-for的基本使用，循环一个数组

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656380625032-cb248840-e3a7-4e98-9d02-87390f08b00f.png#averageHue=%23fcfafa&clientId=u5c458e9f-b0f0-4&from=paste&height=202&id=u74f72d69&originHeight=202&originWidth=346&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24551&status=done&style=none&taskId=ud59c84a5-3d4c-442d-9db3-9439dbcdd74&title=&width=346)

2. v-for：循环对象

第一个参数是对象的属性值<br />第二个参数是对象的属性名<br />第三个参数是下标<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656380787849-fef6846e-6af6-45ad-8f7d-3b71f85278f1.png#averageHue=%23fcfbfa&clientId=u5c458e9f-b0f0-4&from=paste&height=253&id=uf927135d&originHeight=253&originWidth=355&originalType=binary&ratio=1&rotation=0&showTitle=false&size=32781&status=done&style=none&taskId=ud36188b4-cfbf-49ad-9bfe-2a1f2302cdf&title=&width=355)


3. 提高元素的复用，不用每次添加时都更新全部的dom元素，利用:key属性。当元素的key值没有发生改变时那么就不会重新渲染元素

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656381150977-0da63192-fce8-43f9-8784-5f733d98ced2.png#averageHue=%23fcfafa&clientId=u5c458e9f-b0f0-4&from=paste&height=290&id=u74b34685&originHeight=290&originWidth=412&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41556&status=done&style=none&taskId=uf8cd6aae-d654-4c95-8f9a-159ead69461&title=&width=412)

数组的变更函数<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656383684863-07a462bb-64c5-4c70-95e8-b7f6b39e3625.png#averageHue=%23fefefd&clientId=uf450a90f-6036-4&from=paste&height=212&id=ud726557c&originHeight=212&originWidth=451&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58875&status=done&style=none&taskId=u6637573d-8b67-4d8b-903b-c294533dbac&title=&width=451)



4. 添加对象的属性

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656384264183-3c5eddf6-bbe5-443a-928f-335f3b3de6fd.png#averageHue=%23faf8f8&clientId=uf450a90f-6036-4&from=paste&height=180&id=u4c64ba9b&originHeight=180&originWidth=489&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41715&status=done&style=none&taskId=u57b6d2d1-577f-4a64-8038-de67fcd3e2d&title=&width=489)


5. 直接循环数字

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656384360628-0942f9e3-6e9b-470f-a2a7-744b330e9505.png#averageHue=%23e5d0cd&clientId=uf450a90f-6036-4&from=paste&height=25&id=u69c60a3a&originHeight=25&originWidth=340&originalType=binary&ratio=1&rotation=0&showTitle=false&size=8458&status=done&style=none&taskId=u10d58120-4c40-41c3-b0c8-cfe64706723&title=&width=340)


6. 条件+循环渲染，循环和条件不要写在一个标签上！

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656384508053-dd879b5c-ac16-4d7c-b0fb-7bc125acbbc0.png#averageHue=%23fbf9f9&clientId=uf450a90f-6036-4&from=paste&height=206&id=u161b43ba&originHeight=206&originWidth=400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36189&status=done&style=none&taskId=u83451124-a731-4950-b174-b2f2dd23ca6&title=&width=400)

7. template占位符，减少页面上标签臃肿

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656384682175-c6eecf3d-e599-47d9-9d58-64afb79b5781.png#averageHue=%23fbf8f8&clientId=uf450a90f-6036-4&from=paste&height=215&id=u079b1427&originHeight=215&originWidth=453&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41939&status=done&style=none&taskId=u91e10acc-8d1d-472b-bfc3-a38c439a426&title=&width=453)



<a name="Ua3H7"></a>
## 2.12 事件绑定

1. 绑定一个方法

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656385502598-21594295-9ae3-4e50-a2b3-a0df038aaef1.png#averageHue=%23fbfafa&clientId=uf450a90f-6036-4&from=paste&height=292&id=u5084d1c9&originHeight=292&originWidth=504&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36077&status=done&style=none&taskId=u894e7533-47fd-407c-bf00-19cbc9c534b&title=&width=504)

2. 直接写表达式，能写的很少，还是建议用方法

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656385561108-1d399988-bd07-465f-8a04-a3deefc8af34.png#averageHue=%23fbf9f8&clientId=uf450a90f-6036-4&from=paste&height=108&id=ucb523df3&originHeight=108&originWidth=442&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14340&status=done&style=none&taskId=u67732091-0305-46eb-82b3-0d4bd18294a&title=&width=442)

3. 获取原生的事件

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656385645019-e08c3241-43fa-41f9-87e7-96da1a8b66c7.png#averageHue=%23f8f8f7&clientId=uf450a90f-6036-4&from=paste&height=106&id=uf507325a&originHeight=106&originWidth=277&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14992&status=done&style=none&taskId=ua9bcb38a-9cfe-42a1-86c2-95c71f28f5e&title=&width=277)

4. 给事件方法传递形参，但是这样子怎么获取event？

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656385708806-b44be1a5-093f-431f-ab9b-9310c2a02df1.png#averageHue=%23fcfbfb&clientId=uf450a90f-6036-4&from=paste&height=193&id=u84f4ab52&originHeight=193&originWidth=436&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27585&status=done&style=none&taskId=uc466b061-4fa6-4c21-9ce9-4e74aa70dea&title=&width=436)

5. event不作为第一个参数

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656385780302-4f7b3d50-283f-4ca8-a4a2-6400d2f09b1f.png#averageHue=%23fcfafa&clientId=uf450a90f-6036-4&from=paste&height=188&id=u800c7ef8&originHeight=188&originWidth=569&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29720&status=done&style=none&taskId=u1221f011-346a-48cd-9b0c-39a00817f69&title=&width=569)

6. 绑定多个函数，逗号分隔，且加括号。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656385903756-417854bc-2275-4d1f-86fd-371be8b04ae5.png#averageHue=%23fbf8f8&clientId=uf450a90f-6036-4&from=paste&height=104&id=u0b227ea9&originHeight=104&originWidth=599&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18729&status=done&style=none&taskId=u7910ea74-99ef-4d10-bc09-3d087f28798&title=&width=599)

DOM事件流（event flow ）：<br />存在三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段。




事件冒泡(event capturing） ：<br />当一个元素接收到事件的时候 会把他接收到的事件传给自己的父级，一直到window 。（注意这里传递的仅仅是事件 并不传递所绑定的事件函数。所以如果父级没有绑定事件函数，就算传递了事件 也不会有什么表现 但事件确实传递了。）

事件捕获（event capturing）：<br />当鼠标点击或者触发dom事件时，浏览器会从根节点开始由外到内进行事件传播，即点击了子元素，如果父元素通过事件捕获方式注册了对应的事件的话，会先触发父元素绑定的事件。

```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../vue.js"></script>
</head>
<body>
    <div id="root"></div>
</body>
<script>
    var app = Vue.createApp(
        {
            data(){
                return{
                    counter:0
                }
            },
        methods:{
                handleClick(){
                    this.counter +=1;
                },
            handleClickOne(event){
                this.counter +=1;
                console.log(event);
                console.log(event.target);
            },
            handleClickTwo(number){
                    this.counter += number;
            },
            handleClickThree(number,event){
                this.counter += number;
                console.log(event)
            },
            handlerClickFour(){
                    alert("handlerClickFour");

            },
            handleClickFive(){
                alert("handlerClickFive");

            },
            handleDivClick(event){
                    alert("handleDivClick");
                    console.log(event,"handleDivClick");
                    console.log(event.target)

            }
        },
            //事件绑定的第一种方式，绑定一个函数
            // template:`<div>{{counter}}</div>
            //           <button @click="handleClick">add</button>`

            //事件绑定的第二种方式，绑定一个表达式
            // template:`<div>{{counter}}</div>
            //           <button @click="this.counter+=1">add</button>`

            //事件绑定的函数的第一个参数是原生的事件本身
            // template:`<div>{{counter}}</div>
            //           <button @click="handleClickOne">add</button>`

            //事件绑定的函数也可以传入形参，此时第一个参数不是事件本身了
            // template:`<div>{{counter}}</div>
            //           <button @click="handleClickTwo(2)">add</button>`

            //事件绑定的函数传入形参时，如何传入事件？
            // template:`<div>{{counter}}</div>
            //           <button @click="handleClickThree(2,$event)">add</button>`

            //事件绑定可以绑定多个函数，此时不能写函数的引用而是需要写上函数的括号
            // template:`<div>{{counter}}</div>
            //           <button @click="handleClickThree(2,$event),handlerClickFour()">add</button>`

            //事件的冒泡,子标签的事件本身会传递到父标签直到window,在父级标签内也可以查看该时间的来源是下方的button
            // template:`<div>{{counter}}</div>
            //           <div @click="handleDivClick">
            //           <button @click="handleClickThree(2,$event),handlerClickFour()">add</button>
            //           </div>

            //阻止事件的冒泡，修饰符.stop
            // template:`<div>{{counter}}</div>
            //           <div @click="handleDivClick">
            //           <button @click.stop="handleClickThree(2,$event),handlerClickFour()">add</button>
            //           </div>
            //           `

            //事件的.self修饰符：只要当点击该标签时才会触发事件而点击该标签中其它子元素时不会触发事件
            //比如下方，只有点击第一个counter才会触发div事件，点击子div标签内的counter不会触发事件
            // template:`
            //   <div @click.self="handleDivClick">
            //   {{counter}}
            //   <div>{{counter}}</div>
            //
            //     <button @click="handleClickThree(2,$event)">add</button>
            //   </div>
            //           `

            //阻止事件的默认行为  .prevent,比如表单的提交
            // template:`
            //   <form action="https://www.baidu.com" method="get" @click.prevent>
            //     <button type="submit">提交行为被阻止</button>
            //   </form>
            //           `

            //在没有加capture时，如果点击了button，那么先button-->第二个div--->第一个div
            //加了capture后，如果点击了button，那么先第一个div--->第二个div--->button
            // template:`
            //   <div @click.capture="handleDivClick">
            //   {{counter}}
            //   <div @click="handlerClickFour">
            //     {{counter}}
            //     <button @click="handleClickThree(2,$event)">add</button>
            //   </div>
            //
            //
            //   </div>
            //           `

            //下面这个事件捕获的顺序handlerClickFour--->handleClickFive-->handleDivClick
            /*
            使用了关键字：capture
                1.先冒泡外层带有关键字的事件
                2.外层执行结束之后，往里层执行事件
                3.最后按照从里向外的事件开始执行
             */
            // template:`
            //   <div @click="handleDivClick">
            //   {{counter}}
            //   <div @click.capture="handlerClickFour">
            //     {{counter}}
            //     <button @click="handleClickFive">add</button>
            //   </div>
            //   </div>
            //           `


            //事件只有第一次触发时启动  .once
            template:`
              <div @click.once="handleDivClick">
              {{counter}}
              </div>
                      `
        }
    );
    app.mount("#root");
</script>
</html>
```


@scroll，监听滚动事件

按键事件绑定、鼠标事件绑定、精确修饰符

![7b5e1c4debbf4bac3c564f33ac352f10_.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656412785594-93a71740-0795-4b4a-bf75-4e18e64ff9b9.jpeg#averageHue=%23c2beaa&clientId=u0f23bc6f-dc12-4&from=paste&height=1080&id=u6efc0505&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=346360&status=done&style=none&taskId=ud354f561-73d7-44cf-92d0-2375f970b7c&title=&width=1728)

<a name="AotFX"></a>
## 双向数据绑定

![0f726ce787c9dd99bff8002821e64fb4_.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656412839166-1b654f99-799c-458a-8cba-764f14717f84.jpeg#averageHue=%234a4943&clientId=u0f23bc6f-dc12-4&from=paste&height=935&id=u3dee1f54&originHeight=935&originWidth=1600&originalType=binary&ratio=1&rotation=0&showTitle=false&size=191791&status=done&style=none&taskId=u95464256-97b7-4138-bad0-a34fe0c3c2d&title=&width=1600)
<a name="zdol9"></a>
# ![f116ecbaded4da2c8250d8a35d81366e_.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656412858655-0fc4c776-5b9c-458c-bb57-7cb90a1feb6a.jpeg#averageHue=%234a4944&clientId=u0f23bc6f-dc12-4&from=paste&height=976&id=uf4b9814b&originHeight=976&originWidth=1600&originalType=binary&ratio=1&rotation=0&showTitle=false&size=200562&status=done&style=none&taskId=ufc4dac4f-eb41-4025-b8af-3a9ddb39494&title=&width=1600)


![00021833cd505382a5f221bdc92d8ede_.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656412867468-199a7f7c-0ebc-448b-a3b5-ae071f287f49.jpeg#averageHue=%234a4944&clientId=u0f23bc6f-dc12-4&from=paste&height=946&id=u141d433d&originHeight=946&originWidth=1600&originalType=binary&ratio=1&rotation=0&showTitle=false&size=179616&status=done&style=none&taskId=u3de453fc-4733-469a-a3fc-b8821775c9e&title=&width=1600)<br />数据绑定修饰符 lazy 当鼠标失去焦点才发生更新<br />数据绑定修饰符number，保证输入的数据是number类型
<a name="Lsy3q"></a>
# 

<a name="HFbmV"></a>
# 第三章
<a name="oXka4"></a>
## 组件的定义、复用性、全局组件、局部组件

1. 组件的定义
2. 组件具备复用性
3. 全局组件：只要定义了就可以处处使用，性能不高，但是使用起来简单。名字建议 小写字母单词，中间用横线间隔
4. 局部组件，定义之后需要注册才能使用，性能较高，但是使用起来有些麻烦，建议大写字母开头，驼峰命名
5. 局部组件使用时，要做一个名字和组件间的映射对象，你不写映射vue底层也会自动尝试帮你做映射
```java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../vue.js"></script>
</head>
<body>
    <div id="root"></div>
</body>
<script>
    //{}就是根组件
    //组件具有复用性和独立性
    // app.component定义的是全局组件，组件处处可以使用。可以为根组件所用，也可以为子组件所用
    //根组件中的components属性存放的是局部组件，定义要在app定义之前定义
    const local ={
        data(){
            return{
                count:0
            }
        },
        template:`<div @click="count++">{{count}}</div>`
    }
    const  app = Vue.createApp(


        {
           // components:{"localComponents":local},
            //可以简写为以下
            components:{local},

            template:`
            <div>
                <hello/>
                <world/>

                <counter/>
                <counter/>
                <counter/>
                <counter-parent/>
                <local/>
            </div>`
        }
    );
    //子组件1
    app.component("hello",{
        template:`<div>hello</div>`
    });
    //子组件2
    app.component("world",{
        template:`<div>world</div>`
    });
    //子组件3
    app.component("counter",{
        data(){
            return{
                count:0
            }
        },

        template:`<div @click="count++">{{count}}</div>`
    });
    // app.component定义的是全局组件，组件处处可以使用。可以为根组件所用，也可以为子组件所用
    app.component("counter-parent",{
        template:`<counter/>`
    })
    app.mount("#root")
</script>
</html>
```



<a name="SvkFW"></a>
## 组件间传值及传值校验


1. 通过子组件的属性向子组件传递，写死了属性值
2. 通过v-bind绑定属性向子组件传递，可以在父组件中设置属性值

3. 利用props对象来进行校验


4. required表示必须传这个参数\default表示默认值(当有值的时候默认值被取代）\validator表示校验器

```javascript
<script>

    const app = Vue.createApp(
        {
            data(){
              return{
                  text:"Haha",
                  num:1234,
                  str:"teeeee",
                  obj:new Object(),
                  arr:[1,2,3],
                  alertABC(){
                      alert("abc")
                  }
              }
            },
            template:`
                <div>
                    hello,world
<!--//父子组件传递信息的第一种方式,-->
                    <cp1 message="HELLO,WORLD"></cp1>
<!--//父子组件传递信息的第二种方式-->
                    <cp2 :message="text"></cp2>

                    <cp3 :message="num" :text="str" :boo=true  :o=obj :arr="arr" :f="alertABC" :requiredTest="num"> </cp3>
                </div>`
        }
    );
    app.component("cp1",{
        props:["message"],
        template:`<div>{{message}}</div>`
    });
    app.component("cp2",{
        props:["message"],
        template:`<div>{{message}}</div>`
    });
    app.component("cp3",{
        props:
            {
                //规定传入的message为number类型
                message:Number,
                //规定传入的text是String类型
                text:String,
                //规定传入的boo是Boolean类型
                boo:Boolean,
                //规定传入的o是对象
                o:Object,
                //规定传入的arr是数组
                arr:Array,
                //规定传入的f是个function
                f:Function,

                requiredTest:{
                    required:true,
                    type:Number,
                    default:456,
                    validator:function (value){
                        return value<1000
                    }
                }

            },
        template:`<div>{{typeof message}}</div>
                  <div>{{typeof text}}</div>
                  <div>{{typeof boo}}</div>
        <div>{{typeof o}}</div>
        <div>{{typeof arr}}</div>
        <div>{{typeof f}}  <button @click=f>点击运行这个函数</button></div>
        <div>{{typeof requiredTest}}  {{requiredTest}}</div>
        `
    })

    const vm = app.mount("#root");
</script>
```



单向数据流：父组件可以向子组件传递数据，但是子组件对这个被传过来的数据是只能读而不能改的。但是父组件的数据改变时子组件的数据也会随之改变

```javascript
 const app = Vue.createApp(
        {
            data(){
                return{
                    content:0
                }
            },
            template:`
              <button @click="content++">{{content}}</button>
              <cp1 :params="content"></cp1>`
        }
    );

    app.component("cp1",{
        props:{
            params:{type:Number}
        },
        template:`<button @click="params++">{{params}}</button>`
    });

    app.mount("#root")
```



解决方法：在子组件中定义数据区，将被传过来的数据新赋值给该子组件中的数据区的变量
```javascript
 app.component("cp1",{
        props:{
            params:{type:Number}
        },

        data(){
           return{
               content:this.params
           }
        },
        template:`<button @click="content++">{{content}}</button>`
    });
```



<a name="SKemh"></a>
## Non-props
non props 就是父组件传子组件但是子组件却没有接收到props中。 如果子组件只有一个节点那么这个non props会在页面的标签上展示 如果子组件有多个节点，那么就不会展示。  可以通过inheritAttris属性来选择是否继承这些non props。  
```
<script>

    const app = Vue.createApp({
       
        template:`<div>父组件</div>
                 <son msg="msg"></son>
                
                
        `
    });
    app.component("son",{
        inheritAttrs:false,
        template:`<div>son</div>
        `
    })
    
    app.mount("#root")
</script>
```



最后多个节点时也可以通过v-bind绑定多个non props 也可以指定一个non props

![76ba59ceafd56c9bb3e9eb09c626cbb.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656506708570-f559146e-c8d2-4c60-b164-06ce4f158b0f.jpeg#averageHue=%23cdc7b4&clientId=ua8ee4868-30d6-4&from=paste&height=1080&id=u9816303f&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=332918&status=done&style=none&taskId=ue4aae840-27a7-4667-8110-14ca9a17d14&title=&width=1728)

最后一个小知识点，这些non props也可以在生命周期函数中获取 ，所以如果想获取这non-props属性可以通过this.$attrs来获取

![9ca15ca7b20a4acc134e6bd26985504.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656506727362-e7b34a45-791a-4df1-a6fc-c9370bc32cd4.jpeg#averageHue=%23d5cfbb&clientId=ua8ee4868-30d6-4&from=paste&height=549&id=ud328fbb1&originHeight=549&originWidth=2557&originalType=binary&ratio=1&rotation=0&showTitle=false&size=229340&status=done&style=none&taskId=u50bad9e1-d000-4b4d-b1ba-132c2b5c828&title=&width=2557)

<a name="BdVax"></a>
## 组件间通过事件进行通信



父子组件通过事件进行通信，子组件emit，父组件@<br />![d1a559296f7cda1d9631aac84dda19e.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656506810350-be0110de-24cc-4e22-b80a-101f28a0870f.jpeg#averageHue=%23c6c0ad&clientId=ua8ee4868-30d6-4&from=paste&height=1080&id=ud5e29bcd&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=292933&status=done&style=none&taskId=uf54297b7-0f8b-40aa-a10e-7dd9c24ab18&title=&width=1728)

emits中如果是个对象 还可以做检验

![9fe3d71abde087600277df7e7dcd18e.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656506854686-84624848-fb4d-4568-b929-1c15a412313f.jpeg#averageHue=%23c7c2af&clientId=ua8ee4868-30d6-4&from=paste&height=1080&id=u14ca1942&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=280859&status=done&style=none&taskId=u59173201-22c1-44c6-8593-885d5e59788&title=&width=1728)


通过组件上使用v-model 通过事件通信 ，子组件modelValue<br />![173911dc8d539c64fd9fb496be81912.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656506885459-357e0ecb-34b6-43c4-a008-51e53cb2d260.jpeg#averageHue=%23c5bfac&clientId=ua8ee4868-30d6-4&from=paste&height=1080&id=u2d2dd6ae&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=295968&status=done&style=none&taskId=ud4c5a4bb-ec67-43e0-b6d9-cb8bad16639&title=&width=1728)<br />也可以修改默认的modelValue名字<br />![35d6fbdad112c8096ce4cb4c4e2390d.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656506910204-6872b7c0-ad32-4bc1-b410-cea5cac5d247.jpeg#averageHue=%23c5c0ad&clientId=ua8ee4868-30d6-4&from=paste&height=1080&id=ua9a98de6&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=292600&status=done&style=none&taskId=u8d05c33c-9dbf-4b8c-a923-caea3f2f54a&title=&width=1728)

<a name="xlLTN"></a>
## 组件间双向绑定高级用法

1. 绑定多个属性

![a01556e145d0860a2af868f823b341d.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656507036676-d3a8d004-22e1-4fe3-aa52-1e6b4be1726c.png#averageHue=%23fbf9f9&clientId=ua8ee4868-30d6-4&from=paste&height=355&id=ua05fc7f5&originHeight=355&originWidth=665&originalType=binary&ratio=1&rotation=0&showTitle=false&size=87441&status=done&style=none&taskId=u49fc268f-f442-41d5-b4d6-427644b6a66&title=&width=665)

![1c471496c2f863d208055ae0ff33d92.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656507082313-7e8239cb-4ea7-47ca-997a-b4d48d13b804.png#averageHue=%23fbf9f9&clientId=ua8ee4868-30d6-4&from=paste&height=334&id=u1b006d6b&originHeight=334&originWidth=648&originalType=binary&ratio=1&rotation=0&showTitle=false&size=74912&status=done&style=none&taskId=u6eeb2ea5-0f1f-4208-9fca-3c2eccb1ccb&title=&width=648)


2. v-model自定义修饰符

![1a71b722f2ae87bd915b6d724d91ed7.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656507301530-4832ef80-e3e6-4dfe-9175-42de95bbbaf4.png#averageHue=%23faf8f8&clientId=ua8ee4868-30d6-4&from=paste&height=419&id=u8b026920&originHeight=419&originWidth=453&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71870&status=done&style=none&taskId=u0d4b88ff-46c9-4685-a30c-8d9467bcd02&title=&width=453)


<a name="N4hcz"></a>
## slot插槽语法

1. slot是无法直接绑定事件的

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656507707142-0d49daa9-8391-4220-96f2-9a1e875a203e.png#averageHue=%23fcfaf9&clientId=ua8ee4868-30d6-4&from=paste&height=170&id=u710a3d65&originHeight=170&originWidth=353&originalType=binary&ratio=1&rotation=0&showTitle=false&size=21557&status=done&style=none&taskId=u6579a688-de19-47be-9cfc-9c1b2b2ed8a&title=&width=353)<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656507723672-713628a3-f31d-4326-9cb7-dc578869a1a7.png#averageHue=%23fcfafa&clientId=ua8ee4868-30d6-4&from=paste&height=228&id=uba739190&originHeight=228&originWidth=382&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26814&status=done&style=none&taskId=uef89d9f5-21fc-4a62-9cd4-15d3699a1be&title=&width=382)


不仅可以传标签，还可以传组件

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656507794556-89810216-8d28-41f7-96fe-99f15ffe01eb.png#averageHue=%23fbf9f9&clientId=ua8ee4868-30d6-4&from=paste&height=246&id=u98b2d546&originHeight=246&originWidth=275&originalType=binary&ratio=1&rotation=0&showTitle=false&size=30815&status=done&style=none&taskId=u8fd8f9c1-742d-4fe6-956a-31bd7664e82&title=&width=275)


也可以使用父组件而不是子组件的变量<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656507913917-2edcb53c-f833-4d47-b555-7a621a28ef2d.png#averageHue=%23fbf8f8&clientId=ua8ee4868-30d6-4&from=paste&height=200&id=u330a2f38&originHeight=200&originWidth=279&originalType=binary&ratio=1&rotation=0&showTitle=false&size=25237&status=done&style=none&taskId=u78cee6aa-bad2-47b0-a25d-82ecf6b0238&title=&width=279)

父模板里调用的数据属性，使用的都是父模板里的数据<br />子模板里调用的数据属性，使用的都是子模板里的数据



<a name="RQKX6"></a>
# 第五章
<a name="MZoY0"></a>
## 混入

1. 组件data的优先级高于mixin的data的优先级
2. 生命周期函数先执行mixin里面的再执行组件里的
3. 组件methods中的优先级高于mixin里的
```javascript
<script >
    const myMixin = {
        data(){
            return{
                num:222,
                message:`hello!!`
            }
        },
        methods:{
            handleClick(){
                console.log("myMixin  handleClick")
            }
        },
        mounted(){
            console.log("myMixin  mounted")
        }
    }
    const app = Vue.createApp(
        {
            data(){
                return{
                    num:1
                }
            },
            mixins: [myMixin],
            methods:{
                handleClick(){
                    console.log("handleClick")
                }
            },
            mounted(){
                console.log("app  mounted")
            },
            template:`<div>{{num}}------{{message}}</div>
            <button @click="handleClick">点击</button>`
        }
    );
    app.mount("#root");
</script>
</html>
```

上面的代码创建的是局部mixin，子组件无法获取，全局混入时子组件可以获取

```javascript
<script >
    const myMixin = {
        data(){
            return{
                num:222,
                message:`hello!!`
            }
        }
    }
    const app = Vue.createApp(
        {
            data(){
                return{
                    num:1
                }
            },
            mixins: [myMixin],
           
            template:`<div>{{num}}------{{message}}</div>  
            <child></child>`

        }
    );

    app.component("child",{
        template:`<div>{{message}}</div>`
    });
    app.mixin({data() {
            return {
                message:`hahahah????`
            };
        }})
    app.mount("#root");
</script>
</html>
```

1. 一个小的知识点：options属性获取根组件的一般属性


```javascript
 const app = Vue.createApp(
        {
            content:`app content!!`,
            template:`
            <div>{{this.$options.content}}</div>
            `

        }
    );
```

2. 组件中的options属性优先级大于mixin的options属性优先级

```javascript
<script >
    const myMixin = {
        content:` myMixin  content!!`,
    }
    const app = Vue.createApp(
        {
            //content:`app content!!`,        
            mixins: [myMixin],         
            template:`
            <div>{{this.$options.content}}</div>
            `

        }
    );
    app.mount("#root");
</script>
```

3. 改变options的优先级策略
```javascript
<script >
    const myMixin = {
        content:` myMixin  content!!`,       
    }
    const app = Vue.createApp(
        {
            content:`app content!!`,          
            mixins: [myMixin],                   
            template:`
            <div>{{this.$options.content}}</div>
            `
        }
    );
   
    app.config.optionMergeStrategies.content=(mixVal,appVal)=>{
        return mixVal   ||  appVal
    }
    app.mount("#root");
</script>
```

<a name="v8UbK"></a>
## 自定义指令

directive，下面的意思是当input组件挂载之后执行directive中的mounted函数。下面是个全局指令
```javascript
<script>
    const app = Vue.createApp(
        {
            template:`<input v-focus /> `
        }
    );
    app.directive("focus",{
        mounted(el){
            el.focus();
        }
    })
    app.mount("#root");
</script>
```
下面是局部指令
```javascript
<script>
    const directive2 = {
        focus2:{
            mounted(el){
                el.focus();
            }
        }
    }
    const app = Vue.createApp(
        {
            directives:directive2,
            template:`<input v-focus2 /> `
        }
    );
    app.directive("focus1",{
        mounted(el){
            el.focus();
        }
    })
    app.mount("#root");
</script>
```
这些指令除了mounted生命周期函数之外，和组件一样，也有其它的生命周期函数

![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656774170571-9a5443d4-07b0-454d-8839-55c202da5f67.png#averageHue=%23f9f8f7&clientId=u8d155376-fd78-4&from=paste&height=523&id=ufad6769c&originHeight=523&originWidth=535&originalType=binary&ratio=1&rotation=0&showTitle=false&size=106488&status=done&style=none&taskId=ue6d9c421-b8c3-4820-a692-fc89b48685b&title=&width=535)



指令的第二个参数
```javascript
    <style>
        .header{
            position: absolute;
        }
    </style>


<script>
    const app = Vue.createApp(
        {
            template:`
                <div v-test="200" class="header">
                    <input   />
                </div>
                     `
        }
    );
    app.directive("test",{
        mounted(el,bind){
            el.style.top = bind.value+'px';
        }
    });
    app.mount("#root")
</script>
```

第二个参数的arg属性<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656774924966-4bde01ba-c2cf-4b31-bf92-aa8adc395c58.png#averageHue=%23fdfcfc&clientId=u8d155376-fd78-4&from=paste&height=472&id=u6d4bbe8d&originHeight=472&originWidth=862&originalType=binary&ratio=1&rotation=0&showTitle=false&size=86237&status=done&style=none&taskId=uc9bb873d-bfb1-4eef-92ff-f0bf7779adc&title=&width=862)

<a name="MARIN"></a>
## 传送门
将下述的mash类的标签传送到body或者id为hello的标签下

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        .area{
            position: absolute;
            left: 50%;
            top:50%;
            transform: translate(-50%,-50%);
            width: 400px;
            height: 300px;
            background: green;
        }
        .mask{
            position: absolute;
            top:0;
            bottom: 0;
            left: 0;
            right: 0;
            background: black;
            opacity: 0.5;
        }
    </style>
    <script src="../vue.js"></script>
</head>
<body>
    <div id="root"></div>
    <div id="hello"></div>
</body>
<script>
    const app = Vue.createApp(
        {
            data(){
                return{
                    show : false
                }
            },
            methods:{
                handleClick(){
                    this.show = !this.show
                }
            },
            template:`<div class="area">
              <button @click="handleClick">按钮</button>
              <teleport to="body">
                <div class="mask" v-show="show"></div>
              </teleport>

            </div>`
        }
    );

    app.mount("#root")
</script>
</html>
```
<a name="qW6he"></a>
## 更加底层的render函数


```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../vue.js"></script>
</head>
<body>
    <div id="root"></div>
</body>
<script>
    //template--> render --> h --> 虚拟DOM (js对象) --->真实dom --> 展示到页面上
    const app = Vue.createApp(
        {
            template: `<my-title :level="3">hello</my-title>`
        }
    );
    app.component("my-title",{
        props:["level"],
        render(){
            const {h} = Vue;
            //第一个参数是标签名，第二个参数是属性，第三个参数是标签的文本内容
            return h(`h`+this.level,{},this.$slots.default())
        }
        // template:`
        //
        //     <h1 v-if="level===1" >
        //         <slot></slot>
        //     </h1>
        //     <h2 v-if="level===2" >
        //         <slot></slot>
        //     </h2>
        //     <h3 v-if="level===3" >
        //         <slot></slot>
        //     </h3>
        // `
    });
    app.mount("#root")
</script>
</html>
```


<a name="xCSoR"></a>
## 插件
install的第一个参数是根组件

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../vue.js"></script>
</head>
<body>
    <div id="root"></div>
</body>
<script>

    let plugin = {
        install(app,options){
            console.log(app,options);
            app.directive("focus",{
                mounted(el){
                    el.focus();
                }
            })
        }
    }
    let app = Vue.createApp({
        data(){

        },
        template:`<div>hello

        <input v-focus>
        </div>`
    });

    app.use(plugin,{name:`pz`});

    app.mount("#root");
</script>
</html>
```
<a name="ecNqB"></a>
## 第五章实战


```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../vue.js"></script>
</head>
<body>
<div id="root"></div>
</body>
<script>
    let app = Vue.createApp({
        data(){
          return{
              name:`dell`,
              age:24
          }
        },

        rules:{
            age:{
                validate:(age)=> age>25,
                message:`to young`
            }
        },
        template:`<div>hello,{{name}}:age={{age}}</div>`
    });
    //混入语法
    app.mixin(
        {
            created(){
                //遍历根组件中的rules对象
                for (let key in this.$options.rules){
                    //获取rules对象属性的值，即age
                    let item = this.$options.rules[key];
                    //监听age，第一个参数是age对象，第二个对象是回调函数，形参是新的值
                    this.$watch(key,(value)=>{
                        //调用age对象的validate方法
                        let result = item.validate(value);
                        if (!result) {
                            console.log(item.message)
                        }
                    })
                }
            }
        }
    )

   let vm= app.mount("#root");
</script>
</html>
```
也可以用插件进行
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script src="../vue.js"></script>
</head>
<body>
<div id="root"></div>
</body>
<script>
    let app = Vue.createApp({
        data(){
          return{
              name:`dell`,
              age:24
          }
        },

        rules:{
            age:{
                validate:(age)=> age>25,
                message:`to young`
            }
        },
        template:`<div>hello,{{name}}:age={{age}}</div>`
    });
    //插件语法
    let plugin = {
        install(app, options) {
            //混入语法
            app.mixin(
                {
                    created(){
                        //遍历根组件中的rules对象
                        for (let key in this.$options.rules){
                            //获取rules对象属性的值，即age
                            let item = this.$options.rules[key];
                            //监听age，第一个参数是age对象，第二个对象是回调函数，形参是新的值
                            this.$watch(key,(value)=>{
                                //调用age对象的validate方法
                                let result = item.validate(value);
                                if (!result) {
                                    console.log(item.message)
                                }
                            })
                        }
                    }
                }
            )
        }
    };
    app.use(plugin)
   let vm= app.mount("#root");
</script>
</html>
```
<a name="lJXhr"></a>
# 第六章
<a name="ePmNA"></a>
## setup函数的使用
setup函数在实例被完全初始化之前也就是created函数之前执行，所以在setup里不能用this，但是外部的this可以调用setup<br />setup函数最后一定要返回一个对象暴漏给外部。这个对象可以被模板直接使用，这个对象可以包含属性和方法。<br />所以之前的data和methods中的东西全部可以放在setup函数中。
```javascript
<template>
  <div @click="change">{{message}}
  </div>

</template>

<script>

export default {
  name: 'App',
  setup(){
    let message = `hello,vue`
    const  change = ()=>{
      message = `changed`
      console.log(`message changed`)
    }
    return{message,change}
  }

}
</script>

<style>

</style>

```
虽然直接返回数据很简单，但是并没有响应式变化（也就是setup中的东西变化了但是页面上的数据没有变化）。为了响应式变化需要引入下面的学习

<a name="CvwjM"></a>
## ref属性和
ref属性可以使得基本数据类型响应式
```javascript
<template>
  <div @click="handleClick">{{age}}</div>
  <div @click="handleClick">{{sex}}</div>
  <input v-model="age">
    //直接写sex即可，不用sex.value
  <input v-model="sex">
</template>

<script>
import {ref} from "vue";
export default {
  name: 'App',
  setup(){
    //没有使用ref，所以并不会双向绑定，所以handleClick中该属性改变后不会影响展示
    let age = 1;
    let sex = ref('man');
    //使用ref,会变得响应式
    let handleClick = ()=>{
      console.log("handleClick")
      console.log(age);
      console.log(sex);
      console.log(sex.value)
    };
    return{
      handleClick,age,sex
    }
  },
  watch:{
    age(newOne,oldOne){
      console.log(newOne,oldOne)
    },
    sex(newOne,oldOne){
      console.log(newOne,oldOne)
    }
  }
}
</script>

```
底层是通过ref使得基本数据类型变为proxy（{value:值}）：上例为sex='man'  ===> sex=proxy{value：`man`}


<a name="Vyy5t"></a>
## _reactive_
reactive可以使得对象类型响应式
```javascript
<template>
  <div @click="change">{{nameObj.name}}
  </div>

</template>

<script>

import {reactive} from "vue";

export default {
  name: 'App',
  setup(){

    let nameObj = reactive({name:'zh',age:21});
    const  change = ()=>{
      nameObj.name=`zp`;
      console.log(`message changed`)
      console.log(nameObj)
    }
    return{change,nameObj}
  }
}
</script>

```
底层是通过reactive使得对象类型变为proxy（{属性:值,属性:值}）：上例为nameObj' ={  } <br />===> nameObj=proxy{属性:值,属性:值}

综上，我们之前学的data和methods中的东西都可以放到setup中并返回出去


<a name="K5wsm"></a>
## redonly

readonly表示该属性只可以读而不可以改
```javascript
<template>
  <input v-model="nameObj.name">
</template>

<script>

import { readonly} from "vue";

export default {
  name: 'App',
  setup(){

    let nameObj = readonly({name:'zh',age:21});

    return{nameObj}
  },
  watch:{
    nameObj(newOne,oleOne){
      console.log(newOne,oleOne)
    }
  }
}
</script>

```
[Vue warn] Set operation on key "name" failed: target is readonly
<a name="VePs8"></a>
## toRefs
toRefs属性可以从响应式对象中提取出响应式属性
```javascript
<template>
  <input v-model="name">

</template>

<script>

import {reactive, toRefs} from "vue";

export default {
  name: 'App',
  setup(){

    let nameObj = reactive({name:'zh',age:21});
    let  {name} = toRefs(nameObj)

    return{name}
  },
  watch:{
    name(newOne,oldOne){
        console.log(newOne,oldOne)
    }
  }
}
</script>
```

<a name="hh4IN"></a>
## toRef属性（不建议）
首先看一个情况，如果我torefs从对象中提取对象没有定义的属性，此时模板中并不能绑定数据

```javascript
<template>
  <input v-model="sex">

</template>

<script>

import {reactive, toRefs} from "vue";

export default {
  name: 'App',
  setup(){

    let nameObj = reactive({name:'pz',age:21});
    let  {sex} = toRefs(nameObj)

    return{sex}
  },
  watch:{
    sex(newOne,oldOne){
        console.log(newOne,oldOne)
    }
  }
}
</script>

<style>

</style>


```

我们可以把torefs改为toref，并且给该参数定义第二个参数并且将大括号去掉！我们就可以从响应式对象提取没有定义的属性
```javascript
<template>
  <input v-model="sex">
  <button @click="handleClick"></button>

</template>

<script>

import {reactive, toRef} from "vue";

export default {
  name: 'App',
  setup(){

    let nameObj = reactive({name:'pz',age:21});
    let  sex = toRef(nameObj,'sex');
    let handleClick= ()=>{console.log(sex)}

    return{sex,handleClick}
  },
  watch:{
    sex(newOne,oldOne){
        console.log(newOne,oldOne)
    }
  }
}
</script>
```
<a name="wxeOn"></a>
## setup函数的第二个参数context

setup函数的第二个参数context一共有三个属性，attrs，emit，slots<br />新建一个组件，在这个组件中写入完整的setup函数（注意，是在子组件中写）
```javascript
<template>
  <div>cc-w</div>
</template>

<script>
export default {
  name: "cc-w",
  setup(props,contex){
    console.log(props);
    console.log(contex.attrs)
  }
}
</script>
```
然后在父组件中调用（发现在template中直接写子组件名字，webstorm会自动帮你导入）。通过查看可以知道attrs就是non-props
```javascript
<template>
    <cc-w app="cc-w"></cc-w>
</template>

<script>
import CcW from "@/components/c1";

export default {
  name: 'App',
  components: {CcW},
  setup(){

    return{}
  }
}
</script>

<style>

</style>

```
console.log(contex.slots.default());发现这是个虚拟dom，利用前面学过的render函数我们可以将该数组进行渲染。如下：
```javascript
<template>
  <div>cc-w</div>
</template>

<script>
import {h} from "vue";

export default {
  name: "cc-w",
  setup(props,contex){

    console.log(contex.slots.default());
    return ()=>h('button', {},contex.slots.default())
  }
}
</script>
```
第三个参数emit的使用如下：<br />子组件
```javascript
<template>
  <div @click="handleClick">cc-w</div>
</template>

<script>
export default {
  name: "cc-w",
  setup(props,contex){
    let handleClick = ()=>{
      contex.emit(`change`);
    }
    return {handleClick}
  }
}
</script>
```
父组件
```javascript
<template>
    <cc-w app="cc-w" @change="changed"></cc-w>
</template>

<script>
import CcW from "@/components/c1";

export default {
  name: 'App',
  components: {CcW},
  setup(){
    let changed = ()=>{
      alert('changed')
    }
    return{changed}
  }
}
</script
```

<a name="ShXhi"></a>
## 重新todoList
![7c96e252ebf581596f3d2f34521cd01.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656843427165-abca8e04-1b00-4a2a-99c5-ee348c69b62a.jpeg#averageHue=%23c5c0ad&clientId=u475753e4-4706-4&from=paste&height=1080&id=u18b7875c&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=354931&status=done&style=none&taskId=u733d86c8-e55a-425c-a5f1-73da6d9766e&title=&width=1728)
<a name="URbqt"></a>
### 进行封装
![19b4a1302250fff4e91b2d7f709e06a.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656843474585-0b4cb98c-43eb-49de-a080-abef105f638a.jpeg#averageHue=%23c5c0ad&clientId=u475753e4-4706-4&from=paste&height=1080&id=ua308b022&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=389224&status=done&style=none&taskId=u47941122-67b3-4a5e-8462-acdbc79b4f2&title=&width=1728)<br />![769253b57ab987df3af3b17ece27e0d.jpg](https://cdn.nlark.com/yuque/0/2022/jpeg/27635678/1656843479553-9df681f4-af74-4a5c-96ba-ff80f9badbd1.jpeg#averageHue=%23c3beab&clientId=u475753e4-4706-4&from=paste&height=1080&id=u550aca08&originHeight=1080&originWidth=1728&originalType=binary&ratio=1&rotation=0&showTitle=false&size=413426&status=done&style=none&taskId=u835aa87e-65d4-4a03-b200-9cd06ebd28a&title=&width=1728)
<a name="pragJ"></a>
## 计算属性
vue中的computed函数 ,可以设置计算属性
```javascript
<template>
    <div>{{count}}</div>
    <div>{{countAddFive}}</div>
    <button @click="changed">add</button>
</template>

<script>


import {computed, ref} from "vue";

export default {
  name: 'App',
  setup(){
    let count = ref(0);
    let changed = ()=>{
      count.value++;
    };
    let countAddFive = computed(()=>{return count.value+5})
    return{count,changed,countAddFive}
  }
}
</script>
```
<a name="ZMOcR"></a>
### 计算属性的get和set方法
get方法，在获取的时候调用的方法<br />set方法，在修改的时候调用的方法

```javascript
<template>
    <div>{{count}}</div>
    <div>{{countAddFive}}</div>
    <button @click="changed">add</button>
</template>

<script>


import {computed, ref} from "vue";

export default {
  name: 'App',
  setup(){
    let count = ref(0);
    let changed = ()=>{
      count.value++;
    };
    let countAddFive = computed(
    {
      get:()=>{
        return count.value+5
      },
      set:()=>{

        return count.value=1
      }

    })

    setTimeout(()=>countAddFive.value=10,3000)
    return{count,changed,countAddFive}
  }
}
</script>
```



<a name="V6Zac"></a>
## 生命周期函数
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>lesson 40</title>
  <script src="https://unpkg.com/vue@next"></script>
</head>
<body>
  <div id="root"></div>
</body>
<script>

  const app = Vue.createApp({
    // beforeMount => onBeforeMount
    // mounted => onMounted
    // beforeUpdate => onBeforeUpdate
    // beforeUnmount => onBeforeUnmount
    // unmouted => onUnmounted
    setup() {
      const {
        ref, onBeforeMount, onMounted, onBeforeUpdate, onUpdated,
        onRenderTracked, onRenderTriggered
      } = Vue;
      const name = ref('dell')
      onBeforeMount(() => {
        console.log('onBeforeMount')
      })
      onMounted(() => {
        console.log('onMounted')
      })
      onBeforeUpdate(() => {
        console.log('onBeforeUpdate')
      })
      onUpdated(() => {
        console.log('onUpdated')
      })
      // 每次渲染后重新收集响应式依赖
      onRenderTracked(() => {
        console.log('onRenderTracked')
      })
      // 每次触发页面重新渲染时自动执行
      onRenderTriggered(() => {
        console.log('onRenderTriggered')
      })
      const handleClick = () => {
        name.value = 'lee'
      }
      return { name, handleClick }
    },
    template: `
      <div @click="handleClick">
        {{name}}
      </div>
    `,
  });
  
  const vm = app.mount('#root');
</script>
</html>
```
<a name="cDjop"></a>
## watch
![4997417c8a5fb77c4dd07aa4acc3839.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656843649842-db21b2e7-c60f-474f-b927-7f1a40c29963.png#averageHue=%23fcfbfb&clientId=u475753e4-4706-4&from=paste&height=557&id=u1b95fd17&originHeight=557&originWidth=730&originalType=binary&ratio=1&rotation=0&showTitle=false&size=140476&status=done&style=none&taskId=u1ef6fefa-9a1e-485a-9818-bceaf1f65cb&title=&width=730)
<a name="O2rOE"></a>
### 监听一个对象
![1d3832d6d075908c19a6dd193dbfd61.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656843671666-25a4cc66-3645-4571-a18a-0d7cd30054d8.png#averageHue=%23fcfbfb&clientId=u475753e4-4706-4&from=paste&height=587&id=udaf540de&originHeight=587&originWidth=814&originalType=binary&ratio=1&rotation=0&showTitle=false&size=173291&status=done&style=none&taskId=u7ee80462-e550-4a95-9783-fbb1e93d13f&title=&width=814)
<a name="lS6uL"></a>
### 监听多个对象
![e3f7dd327c8eddeaebe139248a36cfc.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656843685103-a947febe-e5b4-4625-926b-5320b6e2bc95.png#averageHue=%23fdfcfc&clientId=u475753e4-4706-4&from=paste&height=576&id=ua81d18f2&originHeight=576&originWidth=991&originalType=binary&ratio=1&rotation=0&showTitle=false&size=202682&status=done&style=none&taskId=u843d783d-f027-4a30-8e20-3d48b4511b2&title=&width=991)<br />![796af9cdb9f924d2daf9974f799c072.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656843695708-59ef57f7-caee-48f2-9f78-10c4eeaa5dfa.png#averageHue=%23fbfbfb&clientId=u475753e4-4706-4&from=paste&height=90&id=u9a8c0fec&originHeight=90&originWidth=490&originalType=binary&ratio=1&rotation=0&showTitle=false&size=24837&status=done&style=none&taskId=u3fd43ccc-e7cd-4e72-a373-a0d35b3f68f&title=&width=490)

<a name="ipwBe"></a>
## watchEffect
![c0683f5a3b9ce9a002d8bc1a122168a.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1656843701936-d25fb7f2-6f88-4541-8da9-13cc6b269173.png#averageHue=%23fdfdfd&clientId=u475753e4-4706-4&from=paste&height=574&id=u9a7936ee&originHeight=574&originWidth=963&originalType=binary&ratio=1&rotation=0&showTitle=false&size=300360&status=done&style=none&taskId=ua5055ec5-fb41-4ff2-afc3-73d86468e61&title=&width=963)
<a name="FgwZp"></a>
# 第七章
<a name="Yft2D"></a>
## VueCLI的使用

1. 安装node
2. 更改镜像源：<br />1）安装nrm,  npm  install nrm -g

2） nrm ls<br />3） nrm use taobao

3. 删掉老版本的vue-cli：npm uninstall vue-cli -g
4. 安装：npm install -g @vue/cli
5. 在当前目录下新建工程： vue create demo
6.  $ cd vuedemo

 $ npm run serve

<a name="Jl5w9"></a>
## 查看工程
src存放的是源代码，入口是main.js文件

```javascript
//从vue中导入creatApp
import {createApp} from 'README'
//从当前路径下的App.vue导入App
import App from './App.vue'
//创建一个根组件App，并且将该根组件挂载到id为app的标签下
createApp(App).mount('#app')
```
App.vue是个组件，如下，有三部分：<template>（模板）<script>（逻辑）<style>（样式）。 只是把之前组件中的template放到了外面的<template>标签下。  App.vue这个组件的name是app，它有一个子组件叫做HelloWorld，这个组件是从./components/HelloWorld.vue导入的组件
```javascript
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>

</style>

```
HelloWorld组件的内容如下：
```javascript
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

```
单文件组件：这个文件就代表了一个组件<br />import的东西不一定要和组件的name相同

实战：todolist
```javascript
<template>
  <div>
    <input v-model="inputValue">
    <button @click="handleClick">提交</button>
    <li v-for="item in list" :key="item">{{item}}</li>
  </div>

</template>

<script>

import {reactive, ref} from "vue";
export default {
  name: 'App',
  setup(){
    const inputValue = ref('');
    const list = reactive([]);
    const handleClick = ()=>{
      list.push(inputValue.value);
      inputValue.value=''
    }

    return {list,handleClick,inputValue}
  }
}
</script>

<style>

</style>

```

使用子组件进行编写

```javascript
<template>
  <div>
    <input v-model="inputValue">
    <button @click="handleClick">提交</button>
    <List v-for="item in list" :key="item" :msg="item"></List>
  </div>

</template>

<script>

import {reactive, ref} from "vue";
import List from "./components/list"
export default {
  name: 'App',
  components: {List},
  setup(){
    const inputValue = ref('');
    const list = reactive([]);
    const handleClick = ()=>{
      list.push(inputValue.value);
      inputValue.value=''
    }

    return {list,handleClick,inputValue}
  }
}
</script>

<style>

</style>
```




<a name="TKprw"></a>
## 路由
根据url的不同显示不同的内容

1. 新建vue工程，选上router
2. main.js中使用了router，导入了router文件夹下的index.js

```javascript
import {createApp} from 'README'
import App from './App.vue'
import router from './router'
import store from './store'

createApp(App).use(store).use(router).mount('#app')
```

3. router文件夹下有index.js
```javascript
import { createRouter, createWebHashHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
]

const router = createRouter({
  history: createWebHashHistory(),
  routes
})

export default router
```

4. App.vue是页面的起始，规定了routerLink和routerView
```javascript
<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>
  <router-view/>
</template>
```

5. 添加一个路由

      1. 创建一个组件，叫做LoginView.vue,注意，vue文件需要是多单词首字母大写
```vue
<template>
  <div>Login</div>
</template>

<script>
  export default {
    name: 'LoginView',

  }
</script>
```
	2. 在路由中配置该组件
```vue
import LoginView from '../views/Login.vue'
{
    path: '/login',
    name: 'login',
    component: LoginView
  },
```
	3. 在app.vue中使用该组件
```vue
<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link> |
    <router-link to="/login">Login</router-link>
  </nav>
  <router-view/>
</template>
```

6. 懒加载路由

懒加载的话访问首页不会将所有的路由都加载(下面的第二种方式)  
```vue
  {
    path: '/login',
    name: 'login',
    component: LoginView
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
```
<a name="PqzCX"></a>
## vuex
首先创建项目，选上所需要的依赖。创建好项目之后看看package.json文件中的内容
```vue
  "dependencies": {
    "core-js": "^3.8.3",
    "vue": "^3.2.13",
    "vue-router": "^4.0.3",
    "vuex": "^4.0.0"
  },
```
相比于之前的项目，有vuex后项目多了store文件夹
```vue
import { createStore } from 'vuex'

export default createStore({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```

在main.js中也使用了store
```vue
createApp(App).use(store).use(router).mount('#app')
```

为什么在一个项目中要使用vuex？vuex是数据管理框架，它创建了一个全局唯一的仓库store用来存放全局的数据<br />使用示例<br />首先在store中的state中定义一个name属性
```vue
import { createStore } from 'vuex'

export default createStore({
  state: {
    name:"dell"
  },
  getters: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```

然后在homeview这个组件中使用这个属性，通过$store可以获取store的state属性进而获取name属性
```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
   {{name}}
  </div>
</template>

<script>


export default {
  name: 'HomeView',
  computed:{
    name(){
      return this.$store.state.name
    }
  }
}
</script>
```
<a name="IWwBN"></a>
### 修改全局数据

1. 想改变数据，vuex要求第一步必须派发(dispatch)一个action，这个action叫做change
```vue
  methods:{
    handleClick(){
      this.$store.dispatch('change')
    }
  }
```

2. 在store的actions中定义change这个action
```vue
  actions: {
    change(){
      console.log(1231313)
    }
  },
```

3. 在action中提交一个commit，触发一个mutation
```vue
  mutations: {
    change(){
      console.log("mutations")
    }
  },
  actions: {
    change(){
      this.commit('change')
    }
  },
```

4. 在mutation中修改数据
```vue
  mutations: {
    change(){
      this.state.name="lee"
    }
  },
```
总代码如下
```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
  <div @click="handleClick"> {{name}}</div>
  </div>
</template>

<script>


export default {
  name: 'HomeView',
  computed:{
    name(){
      return this.$store.state.name
    }
  },
  methods:{
    handleClick(){
      this.$store.dispatch('change');

    }
  }
}
</script>




import { createStore } from 'vuex'

export default createStore({
  state: {
    name:"dell"
  },
  getters: {
  },
  mutations: {
    change(){
      this.state.name="lee"
    }
  },
  actions: {
    change(){
      this.commit('change')
    }
  },
  modules: {
  }
})
```
dispatch和action关联，commit和mutation关联<br />vuex中有一个要求：mutation中只允许写同步代码不允许写异步代码，如果要使用异步代码，就在action中使用异步代码

<a name="mpvs6"></a>
### 传递参数修改
记住一句话，action和mutation的第一个参数是state
```vue
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
  <div @click="handleClick"> {{name}}</div>
  </div>
</template>

<script>


export default {
  name: 'HomeView',
  computed:{
    name(){
      return this.$store.state.name
    }
  },
  methods:{
    handleClick(){
      this.$store.dispatch('change',"haha");

    }
  }
}
</script>


import { createStore } from 'vuex'

export default createStore({
  state: {
    name:"dell"
  },
  getters: {
  },
  mutations: {
    change(state,str){
      this.state.name=str
    }
  },
  actions: {
    change(state,str){
      this.commit('change',str)
    }
  },
  modules: {
  }
})
```

<a name="UYSNf"></a>
### compositionAPI中如何使用vuex

1. 使用全局变量，需要使用useStore函数来获取store对象，然后利用toRefs来响应式绑定全局数据

```vue

<script>

import {toRefs} from "README";
import {useStore} from "vuex"

export default {
   name: 'HomeView',
   setup() {
      const store = useStore();


      const {name} = toRefs(store.state);
      const name2 = store.state.name
      return {
         name, name2
      }
   },
   methods: {
      handleClick() {
         this.$store.dispatch('change', "haha");

      }
   }
}
</script>
```

2. 修改全局数据

```vue

<script>

import {toRefs} from "README";
import {useStore} from "vuex"

export default {
   name: 'HomeView',
   setup() {
      const store = useStore();


      const {name} = toRefs(store.state);
      const name2 = store.state.name
      const handleClick = () => {
         store.dispatch('change', "haha");
      }
      return {
         name, name2, handleClick
      }
   },

}
</script>
```
<a name="PzCNM"></a>
## 使用axios
首先安装axios:   npm install axios， 然后再npm run serve<br />然后导入axios:  
```vue
import axios from "axios"
```

使用axios

```vue

<template>
   <div class="home">
      <img alt="Vue logo" src="../assets/logo.png">
      <div @click="handleClick"> {{name}} {{name2}}</div>
      <div @click="handleClick2">点击发送ajax</div>
   </div>
</template>

<script>

import {toRefs} from "README";
import {useStore} from "vuex"
import axios from "axios"

export default {
   name: 'HomeView',
   setup() {
      const store = useStore();


      const {name} = toRefs(store.state);
      const name2 = store.state.name
      const handleClick = () => {
         store.dispatch('change', "haha");
      }
      const handleClick2 = () => {
         axios.get("https://www.fastmock.site/mock/ae8e9031947a302fed5f92425995aa19/jd/api/user/register").then((response) => {
            console.log(response)
         })
      }
      return {
         name, name2, handleClick, handleClick2
      }
   },

}
</script>
```




<a name="EKoQa"></a>
# 第八章

1. 新建一个vue项目，在vscode中安装ESLINT插件和Vetur插件
2. 各个文件夹的介绍：

       node_modules: 项目需要的依赖包，如果不小心删掉可以使用npm install 重新安装<br />       public  : 公共的部分，包含index.html和favicon.ico<br />       src: 最重要的源代码部分。 <br /> assets：存放一些图片等资源文件   <br />components: 存放一些公用的组件<br />router：存放路由配置<br />store：存放vuex<br />views：存放页面类型的组件<br />App.vue: 根组件<br />main.js  程序的入口<br />browserlistrc:  规定兼容哪些浏览器<br />package.json：存放一些依赖的配置<br />eslintrc.js  eslint的设置

3. 将原先的组件都删掉，让项目变得干净一些


4. 安装normalize.css  来抹平不同浏览器对样式的差异

       npm install -g cnpm --registry=[https://registry.npm.taobao.org](https://registry.npm.taobao.org)<br />cnpm install normalize.css --save

5. 在main.js文件中引入normalize.css

        
```
import 'normalize.css'
```

6. 新建一个style文件夹，在此文件夹下新建base.scss文件。设置html的font-size,这样是为了规定rem的大小，1rem=1 font-size  。 并在main.js文件中导入，使其对全局生效
```
html{
    font-size: 100px;
}

import './style/base.scss'
```
<a name="dFTz8"></a>
## 首页底部docker的编写
为了避免字体太大，在base.scss文件中设置html的子标签body的font-size为0.12rem
```
body{
    font-size: .12rem;
}
```
首先，在app.vue中写一个class为docker的div，设置其为flex布局,绝对定位(绝对定位的元素的位置相对于_**最近的已定位祖先元素**_，如果元素没有已定位的祖先元素，那么它的位置相对于_**最初的包含块**_。所以left，bottom都是相对于html元素的) width:100%是将该元素的宽度设置为和已定位的祖先元素一样。height设置它的高度，这个height不包含它的border，   box-sizing: border-box; 是将盒子模型设置宽高时这个宽高表示的是content和padding和border的总
```
<div class="docker">
  
 </div>
 .docker{
 
    display: flex;    
    position: absolute;
    left: 0 ;
    bottom: 0 ;
    width: 100%;
    height: 0.49rem;
    border-top: 1px solid #f1f1f1;
    background: green;
    box-sizing: border-box;
}
```
在div标签中包含四个span子标签
```
 <span class="docker__itme">首页</span>
    <span class="docker__itme">购物车</span>
    <span class="docker__itme">订单</span>
    <span class="docker__itme">我的</span>
```
因为它的父容器是flex布局所以它们会水平摆置，设置它们四个span水平平分，flex:1  并设置文子水平居中
```
.docker__itme{
    flex: 1;
    text-align: center;
}
```
但是有一个问题，其实span并不是水平平分的，它有一个左和右内边距，所以需要在div中设置padding
```
padding: 0 0.18rem;
```
此时div的效果如下<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1661394425778-31cdec51-8898-47f8-991c-090f10f2664e.png#averageHue=%23f5f1e6&clientId=u4e52f665-7f50-4&from=paste&height=191&id=uf62b77f0&originHeight=191&originWidth=232&originalType=binary&ratio=1&rotation=0&showTitle=false&size=6630&status=done&style=none&taskId=u5378965e-6604-4d71-84ca-2e7321bca89&title=&width=232)<br />为了配置图标，可以去iconfont网站下载资源，先新建一个项目jingdong，搜索home，选中想要的之后加入购物车。搜索cart，选中想要的加入购物车<br />搜索order，选中想要的加入购物车。搜索my，选中想要的加入购物车。<br />点击购物车，下载代码。<br />复制css文件的以下内容
```
@font-face {
  font-family: "iconfont"; /* Project id  */
  src: url('iconfont.ttf?t=1661395045313') format('truetype');
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```
将购物车里的内容添加到刚才新建的jingdong项目中，点击生成代码，将上面的font-face的内容替换成以下的内容
```
@font-face {
  font-family: 'iconfont';  /* Project id 3609664 */
  src: url('//at.alicdn.com/t/c/font_3609664_ote78enkqu.woff2?t=1661395124891') format('woff2'),
       url('//at.alicdn.com/t/c/font_3609664_ote78enkqu.woff?t=1661395124891') format('woff'),
       url('//at.alicdn.com/t/c/font_3609664_ote78enkqu.ttf?t=1661395124891') format('truetype');
}
```

在style文件夹下新建一个icon.css文件，内容如下
```
@font-face {
    font-family: 'iconfont';  /* Project id 3609664 */
    src: url('//at.alicdn.com/t/c/font_3609664_ote78enkqu.woff2?t=1661395124891') format('woff2'),
         url('//at.alicdn.com/t/c/font_3609664_ote78enkqu.woff?t=1661395124891') format('woff'),
         url('//at.alicdn.com/t/c/font_3609664_ote78enkqu.ttf?t=1661395124891') format('truetype');
  }
  
  .iconfont {
    font-family: "iconfont" !important;
    font-size: 16px;
    font-style: normal;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
```
在main.js中导入这个文件
```
import '@/style/icon.css'
```
之后使用这个css文件，在span标签中建立div如下，class为iconfont，内容是项目中点击图标复制的内容
```
 <span class="docker__itme">
        <div class="iconfont">&#xe600;</div>
        首页</span>
    <span class="docker__itme">
        <div class="iconfont">&#xe763;</div>
        购物车</span>
    <span class="docker__itme">
        <div class="iconfont">&#xe713;</div>
        订单</span>
    <span class="docker__itme">
        <div class="iconfont">&#xe635;</div>
        我的</span>
```
在docker__itme中设置图片的字体大小，并设置外边距上0.07rem 右0 下0.02rem 左0
```
.docker__itme{
    flex: 1;
    text-align: center;
    .iconfont{
        margin: 0.07rem 0 0.02rem 0;
        font-size: .2rem;
    }
}
```
为了设置图标下面的字体的大小，我们需要将这些字体包装到一个div中
```
 <div class="docker">
    <span class="docker__itme">
        <div class="iconfont">&#xe600;</div>
        <div class="docker__title">首页</div>
    </span>
    <span class="docker__itme">
        <div class="iconfont">&#xe763;</div>
        <div class="docker__title">购物车</div>
    </span>
    <span class="docker__itme">
        <div class="iconfont">&#xe713;</div>
        <div class="docker__title">订单</div>
    </span>
    <span class="docker__itme">
        <div class="iconfont">&#xe635;</div>
        <div class="docker__title">我的</div>
    </span>
 </div>
```
设置它们的字体大小为10px，因为浏览器最小展示的是12px，所以我们不能直接设置，我们可以通过transfor来缩小0.5倍
```
.docker__title{
    font-size: 20px;
    transform: scale(0.5,0.5);
    transform-origin: center top;
}
```
BEM命名规范：<br />block__element--modifier

因为我们在选中首页的时候颜色会变深，所以我们要对第一个元素进行设置，首先给第一个标签一个新的class，按照bem命名
```
   <span class="docker__itme docker__itme--active">
        <div class="iconfont">&#xe600;</div>
        <div class="docker__title">首页</div>
    </span>
```
	然后设置它的颜色即可
```
.docker__item--active{
    color: #1fa4fc;
}
```
代码优化，因为我们使用的scss，可以将一些代码合并起来<br />比如，
```
.docker{
    display: flex;
    position: absolute;
    left: 0 ;
    bottom: 0 ;
    width: 100%;
    height: 0.49rem;
    border-top: 1px solid #f1f1f1;
    box-sizing: border-box;
    padding: 0 0.18rem;
}
.docker__itme{
    flex: 1;
    text-align: center;
    .iconfont{
        margin: 0.07rem 0 0.02rem 0;
        font-size: .18rem;
    }
}
```
可以写成如下，其中&__itme的意思就是.docker__itme的意思。相当于docker下的itme元素
```
.docker{
    display: flex;
    position: absolute;
    left: 0 ;
    bottom: 0 ;
    width: 100%;
    height: 0.49rem;
    border-top: 1px solid #f1f1f1;
    box-sizing: border-box;
    padding: 0 0.18rem;
    &__itme{
         flex: 1;
         text-align: center;
        .iconfont{
            margin: 0.07rem 0 0.02rem 0;
            font-size: .18rem;
            }
        }
}
```
还有一个优化的地方：在main.js中引入的css文件太多了<br />在style文件夹下新建一个index.scss文件
```
@import './base.scss';
@import './icon.css'
```
然后将原先main.js中引入的两个css文件改成引入一个index.scss文件即可
```
import './style/index.scss'
```
<a name="JO0EG"></a>
## 顶部
在docker上部写一个div并设置样式:绝对定位，上左右皆为0，距离底部0.50rem.并设置左右内边距为0.18rem
```
<div class="wrapper"></div
.wrapper{
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0.50rem;
    background: blue;
    padding: 0 0.18rem;
}
```
在wrapper内部写一个class为position的div，设置其上下内边距为0.16rem，左右内边距为0，内容为北京理工大学国防科技园2号楼10层
```
<div class="position">北京理工大学国防科技园2号楼10层</div>
.position{
    padding: 0.16rem 0;
    line-height: 0.22rem;
    font-size: 0.16rem;
}
```
添加position icon 和bell icon，在position中写两个span来包装icon
```
 <span class="iconfont">&#xe7f1;</span>
        北京理工大学国防科技园2号楼10层
 <span class="iconfont">&#xe887;</span>
```
为了设置icon的大小，给第一个icon设置一个新的class position__icon，但是发现使用 &__icon并没有效果，需要改成.position这样的两层结构	
```
<span class="iconfont position__icon">&#xe7f1;</span>
.position{
    padding: 0.16rem 0;
    line-height: 0.22rem;
    font-size: 0.16rem;
     .position__icon {
        position: relative;
        top: 0.01rem;
        margin-right: 0.08rem;
        font-size: 0.2rem;
    }
}
```
给bell设置一个新的class，position__notice并设置样式
```
 <span class="iconfont position__notice">&#xe887;</span>
 .position{
    position: relative;
    padding: 0.16rem 0;
    line-height: 0.22rem;
    font-size: 0.16rem;
     .position__icon {
        position: relative;
        top: 0.01rem;
        margin-right: 0.08rem;
        font-size: 0.2rem;
    }
    .position__notice{
        position: absolute;
        right: 0rem;
        top: 0.01rem;
    }
}
```
此时我们发现beall的icon跑到上面去了，这是为什么呢？因为我们的position是有padding的，相对于padding来说的确是距离padding0.01rem，所以我们需要加上padding的0.16rem使其称为0.17rem<br />![image.png](https://cdn.nlark.com/yuque/0/2022/png/27635678/1661413114262-6119fa02-5b29-40ad-867f-6ab85423b2ac.png#averageHue=%23202ae1&clientId=u4e52f665-7f50-4&from=paste&height=85&id=u957ddbfa&originHeight=85&originWidth=372&originalType=binary&ratio=1&rotation=0&showTitle=false&size=3397&status=done&style=none&taskId=u94ec0818-b472-416d-a409-2ebe93f165b&title=&width=372)

现在有一个问题，如果地址很长的话就会导致错行覆盖，我们希望在地址很长时显示省略号<br />在position中添加
```
 overflow: hidden;
```
代码优化:  在style文件夹下新建一个viriables.scss文件，其内定义一个变量
```
$content-fontcolor:#333
```
然后在style下引入这个文件
```
<style lang="scss">
@import './style/viriables.scss';
```
将color:#333都替换成color:$content-fontcolor<br />代码优化： 在style文件夹下新建一个minxins.scss文件，内容如下，这个就是文字太多时显示...的css样式，我们把它包装起来
```
@mixin ellipsis {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;  
}
```
我们同样的在style中引入它
```
<style lang="scss">
@import './style/viriables.scss';
@import './style/mixins.scss';
```
并且删掉原先的三个代码，替换成
```
@include ellipsis;
```
<a name="ZMq5w"></a>
## 搜索框
在position下方新建一个div如下
```
<div class="search">
        <span class="iconfont">&#xe6ac;</span>
        山姆会员商店优惠商品
    </div>
```
设置这个search的样式
```
.search{
     margin-bottom: 0.12rem;
    line-height: 0.32rem;
    background: #F5F5F5;
    color:#B7B7B7;
    border-radius: 0.16rem;
    font-size: 0.14rem;
    .iconfont{
        padding: 0 0.12rem 0 0.16rem;
        font-size: 0.2rem;
    }
}
```
<a name="vixHs"></a>
## banner
在search下新建一个div如下
```
 <div class="banner">
        <img class="banner__img" src="http://www.dell-lee.com/imgs/vue3/banner.jpg" >
    </div>
```
设置图片的样式
```
.banner{
    &__img{
        width: 100%;
    }
}
这个相当于直接对img设置宽度百分比
.banner__img{
    width: 100;
}
```
解决网速慢的时候图片与其下部的div的抖动：利用padding-bottom
```
.banner{
    height: 0;
    overflow: hidden;
    padding-bottom: 25.4%;
    &__img{
        width: 100%;
    }
}
```
<a name="uP6Mr"></a>
## banner底部的icons区域
在banner底部新建一个div，icons，其内有img和desc
```
 <div class="icons">
        <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
        <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
        <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
    </div>
```
设置它们的样式，设置展示为flex布局这样就可以水平分布了，然后设置flex-wrap为wrap，这样就可以换行了。设置item的宽度为20%
```
.icons{
    display: flex;
    flex-wrap: wrap;
    &__item{
        width: 20%;
    }
}
```
然后设置img的样式，display展示为block，设置外边距，宽和高
```
.icons{
    display: flex;
    flex-wrap: wrap;
    &__item{
        width: 20%;
        &__img{
            display: block;
            margin: 0 auto;
            width: 0.4rem;
            height: 0.4rem;
        }
    }
}
```
<a name="uvK2G"></a>
## 列表
在icons的div下新建另外一个div  class为gap
```
.gap{
    margin: 0 -0.18rem;
    height: .1rem;
    background: #f1f1f1;
}
```
```
    <div class="nearby">
        <h3 class="nearby__title">附近店铺</h3>
        <div class="nearby__item">
            <img src="http://www.dell-lee.com/imgs/vue3/near.png"  class="nearby__item__img" />
            <div class="nearby__content">
                <div class="nearby__content__title">沃尔玛</div>
                <div class="nearby__content__tags">
                    <span class="nearby__content__tag">月售1万+</span>
                    <span class="nearby__content__tag">月售1万+</span>
                    <span class="nearby__content__tag">月售1万+</span>
                </div>
                <p class="nearby__content__highlight">VIP尊享满89元减4元运费</p>
            </div>
        </div>
    </div>
    
    .nearby{
    &__title{
        margin:0.16rem 0 0.02rem 0;
        font-size: 0.18rem;
        font-weight: normal;
        color: $content-fontcolor;
    }
    &__item{
        display: flex;
        padding-top: 0.12rem;
        &__img{
            margin-right: 0.16rem;
            width:0.56rem;
            height: 0.56rem;
        }
    }
    &__content{
        flex: 1;
        padding-bottom: 0.12rem;
        border-bottom: 1px solid $content-bgColor;
        &__title{
            line-height: 0.22rem;
            font-size: 0.16rem;
            color: $content-fontcolor;
        }
        &__tags{
            margin-top: 0.08rem;
            line-height: 0.18rem;
            font-size: 0.13rem;
            color:$content-fontcolor;
        }
        &__tag{
            margin-right: 0.16rem;
        }
        &__highlight{
            margin-top: 0.08rem;
            line-height: 0.18rem;
            font-size: 0.13rem;
            color: #E93B3B;
        }
    }
}
```
然后将nearyby复制五个，但是发现一个问题，docker底部溢出了，其实只要在wrapper底部加一个overflow-y:auto即可，允许wrapper沿着y轴移动<br />这样的话首页就完成了但是有个问题，现在有300多行的代码，维护起来麻烦，接下来会学习如何拆分它们
<a name="i2cAR"></a>
## 首页拆分成多个组件
在views文件夹下新建一个home文件夹，在home文件夹下新建Home.vue，将app.vue的代码复制一份到home.vue中，在style标签上写script标签，内容如下
```
<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Home'
}
</script>
```
对app.vue中的代码进行删减，里面的style内容删掉，dom结构也都可以删掉，然后写一个script标签
```
<template>
 <Home/>
</template>
<script>
import Home from './views/home/Home.vue'
export default {
  name: 'App',
  components: { Home }
}
</script>
```
建议组件首字母大写，以便于区分普通的dom标签

接下来对Home再次拆分<br />拆分成三个组件，拆分顶部组件<br />在home文件夹下新建一个StaticPart.vue，将Home.vue中的position到gap的内容复制进来，同时将css样式也引进
```
<template>
        <div class="position">
        <span class="iconfont position__icon">&#xe7f1;</span>
        北京理工大学国防科技园2号楼10层
          <span class="iconfont position__notice">&#xe887;</span>
    </div>
    <div class="search">
        <span class="iconfont">&#xe6ac;</span>
        山姆会员商店优惠商品
    </div>
    <div class="banner">
        <img class="banner__img" src="http://www.dell-lee.com/imgs/vue3/banner.jpg" >
    </div>
    <div class="icons">
        <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/超市.png"/>
            <p class="icons__item__desc">超市便利</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/菜市场.png"/>
            <p class="icons__item__desc">菜市场</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/水果店.png"/>
            <p class="icons__item__desc">水果店</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/鲜花.png"/>
            <p class="icons__item__desc">鲜花绿植</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/医药健康.png"/>
            <p class="icons__item__desc">医药健康</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/家居.png"/>
            <p class="icons__item__desc">家居时尚</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/蛋糕.png"/>
            <p class="icons__item__desc">烘焙蛋糕</p>
        </div>
         <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/签到.png"/>
            <p class="icons__item__desc">签到</p>
        </div>
        <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/大牌免运.png"/>
            <p class="icons__item__desc">大牌免运</p>
        </div>
        <div class="icons__item">
            <img class="icons__item__img" src="http://www.dell-lee.com/imgs/vue3/红包.png"/>
            <p class="icons__item__desc">红包套餐</p>
        </div>
    </div>
    <div class="gap"></div>
</template>
<script>
export default {
  name: 'StaticPart'
}
</script>
<style lang="scss">
@import '../../style/viriables.scss';
@import '../../style/mixins.scss';
.position{
    @include ellipsis;
    color: $content-fontcolor;
    position: relative;
    padding: 0.16rem 0.24rem 0.16rem 0;
    line-height: 0.22rem;
    font-size: 0.16rem;
     .position__icon {
        position: relative;
        top: 0.01rem;
        margin-right: 0.08rem;
        font-size: 0.2rem;
    }
    .position__notice{
        position: absolute;
        right: 0rem;
        top: 0.17rem;
    }
}
.search{
    margin-bottom: 0.12rem;
    line-height: 0.32rem;
    background: #F5F5F5;
    color:#B7B7B7;
    border-radius: 0.16rem;
    font-size: 0.14rem;
    .iconfont{
        padding: 0 0.12rem 0 0.16rem;
        font-size: 0.2rem;
    }
}
.banner{
    height: 0;
    overflow: hidden;
    padding-bottom: 25.4%;
    &__img{
        width: 100%;
    }
}
.icons{
    margin-top:0.16rem;
    display: flex;
    flex-wrap: wrap;
    &__item{
        width: 20%;
        &__img{
            display: block;
            margin: 0 auto;
            width: 0.4rem;
            height: 0.4rem;
        }
        &__desc{
            margin: 0.06rem 0 0.16rem 0 ;
            text-align: center;
            color:$content-fontcolor;
        }
    }
}
.gap{
    margin: 0 -0.18rem;
    height: .1rem;
    background: $content-bgColor;
}
</style>
```
然后在home.vue中导入该StaticPart组件并使用<br />同样的步骤，对nearby和docker。

经过这一节，我们就把首页改成了三个组件，这样就很好维护了。

<a name="l86QQ"></a>
## v-for精简代码
比如对于docker这个组件，我们可以将图片和文本放进一个列表中，然后v-for循环
```
<template>
<div class="docker">
    <span class="docker__itme docker__itme--active" v-for="(item, index) in dockerList" :key="index">
        <div class="iconfont">{{item.icon}}</div>
        <div class="docker__title">{{item.text}}</div>
    </span>
 </div>
</template>
<script>
export default {
// eslint-disable-next-line vue/multi-word-component-names
  name: 'Docker',
  setup () {
    const dockerList = [
      { icon: '&#xe600;', text: '首页' },
      { icon: '&#xe763;', text: '购物车' },
      { icon: '&#xe713;', text: '订单' },
      { icon: '&#xe635;', text: '我的' }
    ]
    return { dockerList }
  }
}
</script>
```
但是此时有一个问题，icon会转义，解决办法是使用v-html
```
 <div class="iconfont" v-html="item.icon"></div>
```
还有一个问题class="docker__itme docker__itme--active" 应该只有第一个的时候class才等于docker__itme--active，可以这样写，使用表达式
```
<span :class="{'docker__itme':true,  'docker__itme--active': index===0}" v-for="(item, index) in dockerList" :key="index">
```
同样的道理，循环nearby
```
<template>
    <div class="nearby">
        <h3 class="nearby__title">附近店铺</h3>
        <div class="nearby__item" v-for="item in nearbyList" :key="item.imgSrc">
            <img :src="item.imgSrc"  class="nearby__item__img" />
            <div class="nearby__content">
                <div class="nearby__content__title">{{item.contentTitle}}</div>
                <div class="nearby__content__tags">
                    <span class="nearby__content__tag">{{item.contentTag}}</span>
                    <span class="nearby__content__tag">{{item.contentTag}}</span>
                    <span class="nearby__content__tag">{{item.contentTag}}</span>
                </div>
                <p class="nearby__content__highlight">{{item.contentHighlight}}</p>
            </div>
        </div>
    </div>
</template>
<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Nearby',
  setup () {
    const nearbyList = [
      { imgSrc: 'http://www.dell-lee.com/imgs/vue3/near.png', contentTitle: '沃尔玛', contentTag: '月售1万+', contentHighlight: 'VIP尊享满89元减4元运费' },
      { imgSrc: 'http://www.dell-lee.com/imgs/vue3/near.png', contentTitle: '沃尔玛', contentTag: '月售1万+', contentHighlight: 'VIP尊享满89元减4元运费' },
      { imgSrc: 'http://www.dell-lee.com/imgs/vue3/near.png', contentTitle: '沃尔玛', contentTag: '月售1万+', contentHighlight: 'VIP尊享满89元减4元运费' },
      { imgSrc: 'http://www.dell-lee.com/imgs/vue3/near.png', contentTitle: '沃尔玛', contentTag: '月售1万+', contentHighlight: 'VIP尊享满89元减4元运费' },
      { imgSrc: 'http://www.dell-lee.com/imgs/vue3/near.png', contentTitle: '沃尔玛', contentTag: '月售1万+', contentHighlight: 'VIP尊享满89元减4元运费' }
    ]
    return { nearbyList }
  }
}
</script>
```
同样的，对StaticPart部分也可以使用循环进行精简

```
<template>
        <div class="position">
        <span class="iconfont position__icon">&#xe7f1;</span>
        北京理工大学国防科技园2号楼10层
          <span class="iconfont position__notice">&#xe887;</span>
    </div>
    <div class="search">
        <span class="iconfont">&#xe6ac;</span>
        山姆会员商店优惠商品
    </div>
    <div class="banner">
        <img class="banner__img" src="http://www.dell-lee.com/imgs/vue3/banner.jpg" >
    </div>
    <div class="icons">
        <div class="icons__item" v-for="item in iconList" :key="item.iconSrc">
            <img class="icons__item__img" :src="item.iconSrc"/>
            <p class="icons__item__desc">{{item.iconDesc}}</p>
        </div>
    </div>
    <div class="gap"></div>
</template>
<script>
export default {
  name: 'StaticPart',
  setup () {
    const iconList = [
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/超市.png', iconDesc: '超市便利' },
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/菜市场.png', iconDesc: '菜市场' },
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/水果店.png', iconDesc: '水果店' },
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/鲜花.png', iconDesc: '鲜花绿植' },
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/医药健康.png', iconDesc: '医药健康' },
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/家居.png', iconDesc: '家居时尚' },
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/蛋糕.png', iconDesc: '烘焙蛋糕' },
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/签到.png', iconDesc: '签到' },
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/大牌免运.png', iconDesc: '大牌免运' },
      { iconSrc: 'http://www.dell-lee.com/imgs/vue3/红包.png', iconDesc: '红包套餐' }
    ]
    return { iconList }
  }
}
</script>
```








<a name="v2OjV"></a>
## 补充
一个组件的样式不应该影响外部，在style标签上写scoped

<a name="EmELb"></a>
## 
<a name="FVYcM"></a>
# 第九章  
<a name="G7VtZ"></a>
## 登录页面开发
```
<template>
 <div class="wrapper">
    <img  class="wrapper__img " src="http://www.dell-lee.com/imgs/vue3/user.png">
    <div class="wrapper__input">
         <input class="wrapper__input__content" placeholder="请输入手机号"/>
    </div>
    <div class="wrapper__input">
        <input class="wrapper__input__content" placeholder="请输入密码" type="password"/>
    </div>
    <div class="wrapper__login__button">登录</div>
    <div class="wrapper__login__link">立即注册</div>
 </div>
</template>
<script>
export default {
// eslint-disable-next-line vue/multi-word-component-names
  name: 'Login'
}
</script>
<style lang="scss" scoped>
.wrapper{
    position:absolute;
    top:50%;
    left:0;
    right:0;
    transform: translateY(-50%);
    &__img{
        display: block;
        height: 0.66rem;
        width: 0.66rem;
        margin: 0 auto 0.4rem auto;
    }
    &__input{
        padding:0 0.16rem;
        border-radius: 0.06rem;
        margin: 0 0.4rem 0.16rem 0.4rem;
        background: #F9F9F9;
        border: 1px solid rgba(0,0,0,0.1);
        height: 0.48rem;
        &__content{
            width: 100%;
            border: none;
            background: none;
            outline: none;
            line-height: 0.48rem;
            font-size: 0.16rem;
            color: rgba(0,0,0,0.5);
            &::placeholder{
                color: rgba(0,0,0,0.5);
            }
        }
    }
    &__login__button{
        margin:0.32rem 0.4rem 0 0.4rem;
        line-height: 0.48rem;
        background: #0091ff;
        border-radius: 0.04rem;
        color: #fff;
        box-shadow: 0 0.04rem 0.08rem 0 rgba(0,145,255,0.32);
        font-size: 0.16rem;
        text-align: center;
    }
    &__login__link{
        margin-top: 0.2rem;
        text-align: center;
        font-size: 0.14rem;
        color: #777;
    }
}
</style>
```


优化代码：使用router
```
<template>
<router-view/>
</template>
<script>
export default {
  name: 'App'
}
</script>


import { createRouter, createWebHashHistory } from 'vue-router'
import Home from '../views/home/Home'
import Login from '../views/login/Login'

const routes = [{
  path: '/',
  name: 'home',
  component: Home
}, {
  path: '/login',
  name: 'Login',
  component: Login
}]

const router = createRouter({
  history: createWebHashHistory(),
  routes
})

export default router
```


<a name="zNlIS"></a>
## 路由守卫实现登录校验，以及useRouter
我们想要实现，登陆之后就直接访问首页。<br />在router的index.js中，router有一个beforeEach方法，这个方法会在每次访问router中的组件前都会调用<br />它提供了一个回调函数，这个回调函数有三个参数，第一个参数是将要访问的路径，第二个参数是从哪里访问的路径，第三个是通关函数。
```
router.beforeEach((to, from, next) => {
  console.log(to)
  console.log(from)
  next()
})
to
{fullPath: '/login', path: '/login', query: {…}, hash: '', name: 'Login', …}
from
{fullPath: '/', path: '/', query: {…}, hash: '', name: 'home', …}
```
下面这个会造成死循环，比如初次访问登录界面，isLogin为false，那么就永远访问的是Login。
```
router.beforeEach((to, from, next) => {
  if (isLogin) {
    next()
  } else {
    next({ name: 'home' })
  }
})
```
为了避免死循环可以在if中加一个条件

```
router.beforeEach((to, from, next) => {
  if (isLogin || to.name === 'Login') {
    next()
  } else {
    next({ name: 'Login' })
  }
})
```
isLogin取自于本地存储
```
const isLogin = localStorage.isLogin
```
为了模拟登录时本地存储isLogin，给登录按钮绑定事件
```
<div class="wrapper__login__button" @click="handleLogin">登录</div>
setup () {
    const handleLogin = () => {
      localStorage.isLogin = true
    }
    return { handleLogin }
  }
```
不过此时还有一个问题，在登录后没有自动跳转。为了实现自动跳转可以使用router
```
import { useRouter } from 'vue-router'
export default {
// eslint-disable-next-line vue/multi-word-component-names
  name: 'Login',
  setup () {
    const router = useRouter()
    const handleLogin = () => {
      localStorage.isLogin = true
      router.push({ name: 'home' })
    }
    return { router, handleLogin }
  }
}
```

除了上面的beforeEach，也可以在特定的route下实现beforeEnter，beforeEach是访问任何一个网页时都会调用，而beforeEnter是访问特定网页时才会调用的

```
const routes = [{
  path: '/',
  name: 'home',
  component: Home
}, {
  path: '/login',
  name: 'Login',
  component: Login,
  beforeEnter (to, from, next) {
    const isLogin = localStorage.isLogin
    if (isLogin || to.name === 'Login') {
      next()
    } else {
      next({ name: 'Login' })
    }
  }
}]
```
<a name="kt83P"></a>
## 注册页面开发

在router中注册这个组件并且设置逻辑
```
 {
  path: '/register',
  name: 'Register',
  component: Register
}

router.beforeEach((to, from, next) => {
  const isLogin = localStorage.isLogin
  if (isLogin || to.name === 'Login' || to.name === 'Register') {
    next()
  } else {
    next({ name: 'Login' })
  }
})
```
在登录页面上为注册添加js实践
```
<div class="wrapper__login__link" @click="handleRegister">立即注册</div>
 const handleRegister = () => {
      router.push({ name: 'Register' })
    }
```
注册页面和登录页面差不多
```
<template>
 <div class="wrapper">
    <img  class="wrapper__img " src="http://www.dell-lee.com/imgs/vue3/user.png">
    <div class="wrapper__input">
         <input class="wrapper__input__content" placeholder="请输入手机号"/>
    </div>
    <div class="wrapper__input">
        <input class="wrapper__input__content" placeholder="请输入密码" type="password"/>
    </div>
    <div class="wrapper__input">
        <input class="wrapper__input__content" placeholder="确认密码" type="password"/>
    </div>
    <div class="wrapper__login__button" @click="handleLogin">注册</div>
    <div class="wrapper__login__link" @click="handleLoginClick">已有账号去登录</div>
 </div>
</template>
<script>
import { useRouter } from 'vue-router'
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Register',
  setup () {
    const router = useRouter()
    const handleLoginClick = () => {
      router.push({ name: 'Login' })
    }
    return { handleLoginClick, router }
  }
}
</script>
<style lang="scss" scoped>
.wrapper{
    position:absolute;
    top:50%;
    left:0;
    right:0;
    transform: translateY(-50%);//整个wrapper居中
    &__img{
        display: block;
        height: 0.66rem;
        width: 0.66rem;
        margin: 0 auto 0.4rem auto;//水平居中
    }
    &__input{
        padding:0 0.16rem;
        border-radius: 0.06rem;
        margin: 0 0.4rem 0.16rem 0.4rem;
        background: #F9F9F9;
        border: 1px solid rgba(0,0,0,0.1);
        height: 0.48rem;
        &__content{
            width: 100%;
            border: none;
            background: none;
            outline: none;
            line-height: 0.48rem;
            font-size: 0.16rem;
            color: rgba(0,0,0,0.5);
            &::placeholder{
                color: rgba(0,0,0,0.5);
            }
        }
    }
    &__login__button{
        margin:0.32rem 0.4rem 0 0.4rem;
        line-height: 0.48rem;
        background: #0091ff;
        border-radius: 0.04rem;
        color: #fff;
        box-shadow: 0 0.04rem 0.08rem 0 rgba(0,145,255,0.32);
        font-size: 0.16rem;
        text-align: center;
    }
    &__login__link{
        margin-top: 0.2rem;
        text-align: center;
        font-size: 0.14rem;
        color: #777;
    }
}
</style>

```
<a name="ZKvti"></a>
## 使用axios发送mock请求

```
  <div class="wrapper__input">
         <input class="wrapper__input__content" placeholder="请输入用户名" v-model="data.username"/>
    </div>
    <div class="wrapper__input">
        <input class="wrapper__input__content" placeholder="请输入密码" type="password" v-model="data.password"/>
    </div>
    
    
 setup () {
    const data = reactive({
      username: '',
      password: ''
    })
    const router = useRouter()
    const handleLogin = () => {
      axios.post('https://www.fastmock.site/mock/ae8e9031947a302fed5f92425995aa16/jd/api/user/login', {
        username: data.username,
        password: data.password
      }).then(() => {
        alert('成功')
        localStorage.isLogin = true
        router.push({ name: 'home' })
      })
        .catch(() => { alert('失败') })
    }
    const handleRegister = () => {
      router.push({ name: 'Register' })
    }
    return { router, handleLogin, handleRegister, data }
  }
```
<a name="YkbzO"></a>
## 请求函数的封装
先优化一下，使用async 和awit ，try catch

```
const handleLogin = async () => {
      try {
        const result = await axios.post('https://www.fastmock.site/mock/ae8e9031947a302fed5f92425995aa16/jd/api/user/login', {
          username: data.username,
          password: data.password
        })
        if (result?.status === 200) {
          localStorage.isLogin = true
          router.push({ name: 'home' })
        } else {
          alert('登陆失败')
        }
      } catch (e) {
        alert('请求失败')
      }
    }
```




封装axios请求，我们可以把baseURL封装进去
```
import axios from 'axios'

export const post = (url, data = {}) => {
  return new Promise((resolve , reject) => {
    axios.post(url, data, {
      baseURL: 'https://www.fastmock.site/mock/ae8e9031947a302fed5f92425995aa16/jd',
      headers: {
        'Content-Type': 'application/json'
      }
    }).then((response) => { resolve(response) }, (err) => { reject(err) })
  })
}

```
然后在登录页面上导入post函数（要用括号）
```
import { post } from '../../utils/request'
const handleLogin = async () => {
      try {
        const result = await post('/api/user/login', {
          username: data.username,
          password: data.password
        })
        console.log(result)
        if (result?.status === 200) {
          localStorage.isLogin = true
          router.push({ name: 'home' })
        } else {
          alert('登陆失败')
        }
      } catch (e) {
        alert('请求失败')
      }
    }
```
<a name="hX7E2"></a>
### promise是什么？
主要用于异步计算

- resolve作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；<br />reject作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
- promise有三个状态：<br />1、pending[待定]初始状态<br />2、fulfilled[实现]操作成功<br />3、rejected[被否决]操作失败

<a name="dJgA8"></a>
## 弹窗组件开发
弹窗组件
```vue
<template>
   <div class="tanchuang">
    <span class="tanchuang__content">{{message}}</span>
    </div>
</template>
<script>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Toast',
  props: ['message']
}
</script>
<style lang="scss" scoped>
    .tanchuang{
        position: fixed;
        height: 100%;
        width: 100%;
        &__content{
            position: fixed;
            top: 55%;
            left: 45%;
            font-size: 0.20rem;
            transform: translateY(-50%);
            background-color:dimgrey;
            color: beige;
        }
    }
</style>
```
在登录页面
```vue
<template>
 <div class="wrapper">
    <img  class="wrapper__img " src="http://www.dell-lee.com/imgs/vue3/user.png">
    <div class="wrapper__input">
         <input class="wrapper__input__content" placeholder="请输入用户名" v-model="data.username"/>
    </div>
    <div class="wrapper__input">
        <input class="wrapper__input__content" placeholder="请输入密码" type="password" v-model="data.password"/>
    </div>
    <div class="wrapper__login__button" @click="handleLogin">登录</div>
    <div class="wrapper__login__link" @click="handleRegister">立即注册</div>
    <Toast v-show="data.show" :message="data.message"></Toast>
 </div>
</template>
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Login',
  // eslint-disable-next-line no-dupe-keys, camelcase
  components: { Toast },
  setup () {
    const data = reactive({
      username: '',
      password: '',
      show: false,
      message: ''
    })
    const router = useRouter()
    const handleLogin = async () => {
      try {
        const result = await post('/api/user/login', {
          username: data.username,
          password: data.password
        })
        console.log(result)
        if (result?.status === 200) {
          data.show = true
          data.message = '登录成功'
          localStorage.isLogin = true
          setTimeout(() => (router.push({ name: 'home' })), 2000)
        } else {
          data.show = true
          data.message = '登录失败'
          setTimeout(() => {
            router.push({ name: 'Login' })
            data.show = false
            data.message = ''
          }
          , 2000)
        }
      } catch (e) {
        data.show = true
        data.message = '请求失败'
        setTimeout(() => {
          router.push({ name: 'Login' })
          data.show = false
          data.message = ''
        }
        , 2000)
      }
    }
    const handleRegister = () => {
      router.push({ name: 'Register' })
    }
    return { router, handleLogin, handleRegister, data }
  }
```
<a name="B1xvg"></a>
## 通过拆分精简代码
我们首先将弹窗的逻辑抽象出一个函数
```vue
    const showToast = (message) => {
      data.show = true
      data.message = message
      if (message === '登陆成功') {
        setTimeout(() => (router.push({ name: 'home' })), 2000)
      } else {
        data.username = ''
        data.password = ''
        setTimeout(() => (router.push({ name: 'login' })), 2000)
      }
    }
```
那么登录的逻辑将会大大减小
```vue
 const handleLogin = async () => {
      try {
        const result = await post('/api/user/login', {
          username: data.username,
          password: data.password
        })
        console.log(result)
        if (result?.status === 200) {
          showToast('登陆成功')
          localStorage.isLogin = true
        } else {
          showToast('登陆失败')
        }
      } catch (e) {
        showToast('请求失败')
      }
    }
```

进一步优化：<br />将弹窗的逻辑提取到export的外部
```vue
<Toast v-show="toast.show" :message="toast.message"></Toast>

  const toastEfeect = () => {
  const router = useRouter()
  const toast = reactive({
    show: false,
    message: ''
  })
  const showToast = (message) => {
    toast.show = true
    toast.message = message
    if (message === '登陆成功') {
      setTimeout(() => (router.push({ name: 'home' })), 2000)
    } else {
      toast.username = ''
      toast.password = ''
      setTimeout(() => (router.push({ name: 'login' })), 2000)
    }
  }
  return { toast, showToast }
}
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Login',
  // eslint-disable-next-line no-dupe-keys, camelcase
  components: { Toast },
  setup () {
    const data = reactive({
      username: '',
      password: ''
    })
    const { toast, showToast } = toastEfeect()
    const router = useRouter()
    const handleLogin = async () => {
      try {
        const result = await post('/api/user/login', {
          username: data.username,
          password: data.password
        })
        console.log(result)
        if (result?.status === 200) {
          showToast('登陆成功')
          localStorage.isLogin = true
        } else {
          showToast('登陆失败')
        }
      } catch (e) {
        showToast('请求失败')
      }
    }
    const handleRegister = () => {
      router.push({ name: 'Register' })
    }
    return { router, handleLogin, handleRegister, data, toast, showToast }
  }
}
</script>
```
也可以将弹窗的逻辑提取到弹窗组件文件中
```vue
import { useRouter } from 'vue-router'
import { reactive } from '@vue/reactivity'

export const toastEfeect = () => {
  const router = useRouter()
  const toast = reactive({
    show: false,
    message: ''
  })
  const showToast = (message) => {
    toast.show = true
    toast.message = message
    if (message === '登陆成功') {
      setTimeout(() => (router.push({ name: 'home' })), 2000)
    } else {
      toast.username = ''
      toast.password = ''
      setTimeout(() => (router.push({ name: 'login' })), 2000)
    }
  }
  return { toast, showToast }
}
```
然后再在登录组件中导入这个函数
```vue
import Toast, { toastEfeect } from '../../components/TanChuang.vue'
```

<a name="lJ9JY"></a>
## 利用toRefs精简代码
```vue
export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Login',
  // eslint-disable-next-line no-dupe-keys, camelcase
  components: { Toast },
  setup () {
    const data = reactive({
      username: '',
      password: ''
    })
    const { toast, showToast } = toastEfeect()
    const { show, message } = toRefs(toast)
    const { username, password } = toRefs(data)
    const router = useRouter()
    const handleLogin = async () => {
      try {
        const result = await post('/api/user/login', {
          username: data.username,
          password: data.password
        })
        console.log(result)
        if (result?.status === 200) {
          showToast('登陆成功')
          localStorage.isLogin = true
        } else {
          showToast('登陆失败')
        }
      } catch (e) {
        showToast('请求失败')
      }
    }
    const handleRegister = () => {
      router.push({ name: 'Register' })
    }
    return { handleLogin, handleRegister, username, password, show, message }
  }
}
</script>
```
将登录的逻辑也抽离出去
```vue
const userLoginEffect = (showToast) => {
  const data = reactive({
    username: '',
    password: ''
  })
  const { username, password } = toRefs(data)
  // eslint-disable-next-line no-unused-vars
  const handleLogin = async () => {
    try {
      const result = await post('/api/user/login', {
        username: username,
        password: password
      })
      if (result?.status === 200) {
        showToast('登陆成功')
        localStorage.isLogin = true
      } else {
        showToast('登陆失败')
      }
    } catch (e) {
      showToast('请求失败')
    }
  }
  return { username, password, handleLogin }
}

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Login',
  components: { Toast },
  setup () {
    const { toast, showToast } = toastEfeect()
    const { username, password, handleLogin } = userLoginEffect(showToast)
    const { show, message } = toRefs(toast)
    const router = useRouter()
    const handleRegister = () => {
      router.push({ name: 'Register' })
    }
    return { handleLogin, handleRegister, username, password, show, message }
  }
}
</script>
```
setup函数的职责就只是告诉你代码执行的流程而不包含代码逻辑<br />同样的我们还可以把register的逻辑也抽离出去
```vue
const userLoginEffect = (showToast) => {
  const data = reactive({
    username: '',
    password: ''
  })
  const { username, password } = toRefs(data)
  // eslint-disable-next-line no-unused-vars
  const handleLogin = async () => {
    try {
      const result = await post('/api/user/login', {
        username: username,
        password: password
      })
      if (result?.status === 200) {
        showToast('登陆成功')
        localStorage.isLogin = true
      } else {
        showToast('登陆失败')
      }
    } catch (e) {
      showToast('请求失败')
    }
  }
  return { username, password, handleLogin }
}
const userRegisterEffect = () => {
  const router = useRouter()
  const handleRegister = () => {
    router.push({ name: 'Register' })
  }
  return { handleRegister }
}

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Login',
  components: { Toast },
  setup () {
    const { toast, showToast } = toastEfeect()
    const { username, password, handleLogin } = userLoginEffect(showToast)
    const { show, message } = toRefs(toast)
    const { handleRegister } = userRegisterEffect()
    return { handleLogin, handleRegister, username, password, show, message }
  }
}
</script>
```
<a name="ACnkg"></a>
# 第十章
<a name="CCE0n"></a>
## 附近商家信息mock

```vue

<template>
   <div class="nearby">
      <h3 class="nearby__title">附近店铺</h3>
      <div class="nearby__item" v-for="item in resultList" :key="item._id">
         <img :src="item.imgUrl" class="nearby__item__img"/>
         <div class="nearby__content">
            <div class="nearby__content__title">{{item.name}}</div>
            <div class="nearby__content__tags">
               <span class="nearby__content__tag">起送:{{item.expressLimit}}</span>
               <span class="nearby__content__tag">基础运费{{item.expressPrice}}</span>
               <span class="nearby__content__tag">月销:{{item.sales}}</span>
            </div>
            <p class="nearby__content__highlight">{{item.slogan}}</p>
         </div>
      </div>
   </div>
</template>
<script>
import {get} from '@/utils/request'
import {ref} from 'README'

const nearbyListEffect = () => {
   const resultList = ref([])
   const getNearbyList = async () => {
      const result = await get('/api/shop/hot-list', {})
      resultList.value = result.data.data
      console.log(resultList.value)
   }
   return {resultList, getNearbyList}
}

export default {
   // eslint-disable-next-line vue/multi-word-component-names
   name: 'Nearby',
   setup() {
      const {resultList, getNearbyList} = nearbyListEffect()
      getNearbyList()
      return {resultList}
   }
}
</script>
```
require.js
```vue
import axios from 'axios'
const instance = axios.create({
  baseURL: 'https://www.fastmock.site/mock/ae8e9031947a302fed5f92425995aa19/jd/',
  timeout: 10000
})

export const get = (url, params = {}) => {
  return new Promise((resolve, reject) => {
    instance.get(url, { params }).then((response) => { resolve(response) }, (err) => { reject(err) })
  })
}
export const post = (url, data = {}) => {
  return new Promise((resolve, reject) => {
    instance.post(url, data).then((response) => { resolve(response) }, (err) => { reject(err) })
  })
}

```
<a name="tRsEF"></a>
## 动态路由、异步路由与组件拆分复用
在views目录下新建一个shop目录并且shop目录下新建一个Shop组件，并配置router<br />下面使用动态路由来配置Shop组件
```vue
 {
  path: '/',
  name: 'Shop',
  component: import(/* webpackChunkName : "shop_123123" */ '../views/shop/Shop')
}
```
这样我们在访问主页时不会自动加载Shop而是在进入shop页面的时候才会加载Shop

将附近商店信息拆分出一个组件，在components文件夹下新建一个公共组件ShopInfo,将Nearby组件中的商店信息拆分出来

ShopInfo如下
```vue
<template>
  <div class="shop__item" >
    <img :src="item.imgUrl"  class="shop__item__img" />
    <div class="shop__content">
      <div class="shop__content__title">{{item.name}}</div>
      <div class="shop__content__tags">
        <span class="shop__content__tag">起送:{{item.expressLimit}}</span>
        <span class="shop__content__tag">基础运费{{item.expressPrice}}</span>
        <span class="shop__content__tag">月销:{{item.sales}}</span>
      </div>
      <p class="shop__content__highlight">{{item.slogan}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ShopInfo',
  props: ['item']
}
</script>

<style lang="scss" scoped>
@import '../style/viriables.scss';
.shop__item{
  display: flex;
  padding-top: 0.12rem;
  &__img{
    margin-right: 0.16rem;
    width:0.56rem;
    height: 0.56rem;
  }
}
.shop__content{
  flex: 1;
  padding-bottom: 0.12rem;
  border-bottom: 1px solid $content-bgColor;
  &__title{
    line-height: 0.22rem;
    font-size: 0.16rem;
    color: $content-fontcolor;
  }
  &__tags{
    margin-top: 0.08rem;
    line-height: 0.18rem;
    font-size: 0.13rem;
    color:$content-fontcolor;
  }
  &__tag{
    margin-right: 0.16rem;
  }
  &__highlight{
    margin-top: 0.08rem;
    line-height: 0.18rem;
    font-size: 0.13rem;
    color: #E93B3B;
  }
}
</style>

```
NearBy如下

```vue

<script>
import {get} from '@/utils/request'
import {ref} from 'README'
import ShopInfo from '@/components/ShopInfo'

const nearbyListEffect = () => {
   const resultList = ref([])
   const getNearbyList = async () => {
      const result = await get('/api/shop/hot-list', {})
      resultList.value = result.data.data
   }
   return {resultList, getNearbyList}
}

export default {
   // eslint-disable-next-line vue/multi-word-component-names
   name: 'Nearby',
   components: {ShopInfo},
   setup() {
      const {resultList, getNearbyList} = nearbyListEffect()
      getNearbyList()
      return {resultList}
   }
}
</script>
<style lang="scss" scoped>
@import '../../style/viriables.scss';
@import '../../style/mixins.scss';

.nearby {
   &__title {
      margin: 0.16rem 0 0.02rem 0;
      font-size: 0.18rem;
      font-weight: normal;
      color: $content-fontcolor;
   }
}
</style>

```

在商家页面我们也需要ShopInfo，但是有一个问题，这个商家页面有一个border-bottom，我们不想让它在商业详情页面出现，我们可以这样做（动态class属性的应用）
```vue
<template>
  <div class="wrapper">
    <ShopInfo :item="item" hide_border=true></ShopInfo>
  </div>
</template>

<script>
import ShopInfo from '@/components/ShopInfo'

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Shop',
  components: { ShopInfo },
  setup () {
    const item = {
      _id: 1,
      name: '沃尔玛',
      imgUrl: 'http://www.dell-lee.com/imgs/vue3/near.png',
      seals: 10000,
      expressLimit: 0,
      slogan: 'VIP尊享满89元减4元运费券'
    }
    return { item }
  }
}
</script>

<style scoped>
.wrapper{
  padding: 0 0.25rem;
}
</style>

```
在ShopInfo我们新增一个class
```vue
 <div  :class="{'shop__content':true,'shop__content--bordered':hide_border? false: true}">

   .shop__content{
  flex: 1;
  padding-bottom: 0.12rem;
  &--bordered{
    border-bottom: 1px solid $content-bgColor;
  }
```

<a name="vEirw"></a>
## 搜搜框
```vue
<template>
  <div class="wrapper">
    <div class="search">
      <div class="search__back iconfont">&#xe662;</div>
      <div class="search__content">
        <span  class="search__content__icon iconfont">&#xe6ac;</span>
        <input class="search__content__input" placeholder="请输入商品名称"/>
      </div>
    </div>
    <ShopInfo :item="item" :hide_border='true'></ShopInfo>
  </div>
</template>

<script>
import ShopInfo from '@/components/ShopInfo'

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Shop',
  components: { ShopInfo },
  setup () {
    const item = {
      _id: 1,
      name: '沃尔玛',
      imgUrl: 'http://www.dell-lee.com/imgs/vue3/near.png',
      seals: 10000,
      expressLimit: 0,
      slogan: 'VIP尊享满89元减4元运费券'
    }
    return { item }
  }
}
</script>

<style lang="scss" scoped>
.wrapper{
  padding: 0 0.25rem;
}
.search{
  height: 0.32rem;
  margin:0.2rem 0 0.16rem 0;
  display: flex;
  &__back{
    height: 0.32rem;
    width: 0.3rem;
    font-size: 0.3rem;
    line-height: 0.32rem;
    color: #B6B6B6;
  }
  &__content{
    display: flex;
    flex: 1;// 占用剩余的flex空间
    background: #F5F5F5;
    border-radius: 0.16rem;
    line-height: 0.32rem;
    &__icon{
      width: 0.44rem;
      height: 0.32rem;
      text-align: center;
    }
    &__input{
      display: block;
      width: 100%;
      border:none;
      outline: none;
      background: none;
      height: 0.32rem;
      &::placeholder{
        color: #333;
        font-size: 0.14rem;
      }
    }
  }
}
</style>

```

后退事件绑定
```vue
const handleEffect = () => {
  const router = useRouter()
  const handleClickBack = () => {
    router.back()
  }
  return { handleClickBack }
}
```

<a name="M2av5"></a>
## 路由参数的传递以及商家详情的获取
router设置
```vue
{
  path: '/shop/:id',
  name: 'Shop',
  component: import(/* webpackChunkName : "shop_123123" */ '../views/shop/Shop')
}
```
router-link设置
```vue
   <router-link :to="`/shop/${item._id}`" v-for="item in resultList" :key="item._id">
        <ShopInfo  :item="item"></ShopInfo>
      </router-link>
```
"`/shop/${item._id}`" 这是模板字符串，反引号扩起来，拼接变量和字符串


shop界面配置路由传参
```vue
    <ShopInfo :item="data.item" :hide_border='true'></ShopInfo>
```
请求
```vue
const getItem = () => {
  const data = reactive({ item: {} })
  const getItemData = async () => {
    const result = await get('/api/shop/1')
    if (result?.data.errno === 0) {
      data.item = result.data.data
    }
  }
  return { data, getItemData }
}
```

将请求的id带入路由中，获取路由的参数可以由route中的params中获取(不是router，router是为了跳转，route是为了参数获取)
```vue
 const route = useRoute()
  const id = route.params.id

const result = await get(`/api/shop/${id}`)
```
