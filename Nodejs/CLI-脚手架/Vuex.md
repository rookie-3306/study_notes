# Vuex的相关操作.
_Vuex是官方推出的一个插件,用于多页面的状态管理._

## 插件的安装:
- #### 用npm指令安装:
`npm install vuex --save`

## Vuex的初步使用:
- #### 首先在src文件夹下创建一个store(仓库)文件夹,然后创建一个index.js文件:

```js
import Vue from 'vue'
import Vuex from 'vuex'

//1.安装插件
Vue.use(Vuex)

//2.创建对象
const store = new Vuex.Store({
    state: {
        //一般用来存放状态属性的
        counter:1000
    },
    mutations: {

    },
    actions: {

    },
    getters: {
        
    },
    modules: {

    }
})

//3.导出
export default store
```

- #### 在App.vue中导入store并且使用:

```html
<template>
  <div id="app">
    <!-- 读取store中的counter属性 -->
    <h1>{{$store.state.counter}}</h1>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
</style>

```

## Vuex插件的安装与vuex中的state监控:
- #### 首先在Chrome中安装Vue Devtools插件来监控vuex中state的数值变化.
- #### 在Vuex主文件(一般放在router下的index.js文件)中配置mutations属性来监控state中数值的变化:

```js
import Vue from 'vue'
import Vuex from 'vuex'

//1.安装插件
Vue.use(Vuex)

//2.创建对象
const store = new Vuex.Store({
    state: {
        counter:1000
    },
    mutations: {
        //当执行这里面的方法的时候会自动传入state对象
        increment(state){
            state.counter++
        },
        decrement(state){
            state.counter--
        }
    },
    actions: {

    },
    getters: {
        
    },
    modules: {

    }
})

export default store
```

- #### 在App.vue中提交调用上面定义的方法:

```html
<template>
  <div id="app">
    <h1>{{$store.state.counter}}</h1>
    <button @click="addition">+</button>
    <button @click="subtracion">-</button>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    addition(){
      //执行vuex中的mutations中的方法
      //只有用commit(提交)执行才能让vuex插件监控到
      this.$store.commit('increment')
    },
    subtracion(){
      this.$store.commit('decrement')
    }
  },
}
</script>

<style>

</style>

```

# Vuex中的核心概念:

```js
const store = new Vuex.Store({
    state: {

    },
    mutations: {

    },
    actions: {

    },
    getters: {
        
    },
    modules: {

    }
})
```
- #### state:单一状态树,用来存放状态信息.

```js
state: {
        //单一状态树,用来存放状态信息
        counter:1000,
        students:[
            {id:1,name:'张三',age:18},
            {id:2,name:'王五',age:22},
            {id:3,name:'李四',age:20},
            {id:4,name:'他他开',age:17},
            {id:5,name:'芜湖',age:21},
        ]
    }
```

- #### getters:类似于vue中的computed属性,来取出对state操作之后的数据.

```js
getters: {
        //类似于vue中的computed属性
        //当需要对state中的属性进行操作之后再返回一般就写在getters属性中.
        //当执行这里面的方法的时候会自动传入state对象
        powerCounter(state){
             return state.counter * state.counter
        },
        more20stu(state){
            //filter是高阶函数,自己可以去看下
            return state.students.filter((s) =>{
                return s.age >= 20
            })
        },
        //也可以使用其他参数
        more20stuLenght(state,getters){
            return getters.more20stu.length
        },
        //当碰到需要接收参数的时候
        morAgeStu(state){
            //直接返回一个函数,这个函数的参数列表为传进来的参数列表
            return function(age){
                return state.students.filter((s) =>{
                    return s.age >= age
                })
            }
        }
    }
```

- #### mutations:Vuex的store状态更新的唯一方式.

```js
mutations: {
        //当执行这里面的方法的时候会自动传入state对象
        increment(state){
            state.counter++
        },
        decrement(state){
            state.counter--
        },
        //当需要参数的时候，再.commit('addStu', arg1)中写入参数就行
        addStu(state,stu){
            state.students.push(stu)
        },
        //另外一种提交方式,使用载荷
        addCount(state,payload){
            state.counter += payload.count
        }
    }
```

```js
addCount(count){
      //创建载荷提交
      this.$store.commit({
        type:'addCount',
        // count:count,
        count
      })
    },
```

**当碰到需要添加或者删除state中某个属性中的属性的时候,我们也在mutations写方法来操控:**

```js
mutations: {
        ...
        updateInfor(state){
            //用Vue.set()方法就能让新增加的属性受到Vuex的监控
            //三个参数:修改的属性名,添加的属性名,添加的属性值
            Vue.set(state.info,'address','帕拉迪岛')
            //用Vue.delete()方法就能让删除的属性受到Vuex的监控
            Vue.delete(state.info,'age')
        }
        ...
    }
```

- #### actions:Vuex执行异步操作的地方:

```js
actions: {
        //actions:进行异步操作
        //context为上下文对象,这里可以理解为store对象
        aUpdateInfo(context, payload){
            return new Promise((resolve,reject) => {
                setTimeout(() => {
                    context.commit('updateInfor');
                    console.log(payload)
                    //执行then()函数
                    resolve('执行成功!')
                },1000)
            })
        }
    }
```

```js
this.$store.dispatch('aUpdateInfo','我是携带的信息').then((res) =>{
        //执行成功之后的回调函数
        console.log(res)
      })
```

- #### mutations:存放模块管理的地方(套娃):

```js
//创建一个新模块
...
const moduleA = {
    state:{
        wife:{
            name:'雪之下雪乃',
            age:18
        } 
    },
    mutations:{
        pushWifeAddress(state){
            //这里的state指的就是自己,所以没必要state.a.wife
            Vue.set(state.wife,'addresss','家')
        }
    },
    getters:{

    },
    actions:{
        asyncPushWifeAddress(context){
            setTimeout(()=>{
                context.commit('pushWifeAddress')
            },1000)
        }
    }
}
...
const store = new Vuex.Store({
  ...
  modules: {
      //声明模块
      a:moduleA
    }
})
```

调用:

```html
<script>
export default {
  name: 'App',
  methods: {
    ...
    addWifeAddress(){
      this.$store.commit('pushWifeAddress')
    },
    asyncPushWifeAddress(){
      this.$store.dispatch('asyncPushWifeAddress')
    }
  }
}
</script>
```