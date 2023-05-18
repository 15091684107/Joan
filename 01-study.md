# VUE3深度学习笔记 #
## 01-深入组件 ##
### 注册 ###
测试全局注册组件是否成功
```
//main.js里面写的内容
import { createApp } from 'vue'
import HelloWorld from '../src/components/HelloWorld.vue' 
import Test1 from '../src/view/Test1.vue'
import './style.css'
import App from './App.vue'
const app = createApp(App)
app.component('hello',HelloWorld)   //括号里面第一个为注册的名字，第二个为组件的实现
   .component('test1',Test1)

app.mount('#app')

```
如果在App.vue中使用
```
<template>
  
  <hello/>
  <test1/>
  
</template>

```
测试局部注册组件
```
//App.vue
<script>
import Test2 from '../src/view/Test2.vue'
export default {
  components: {
    Test2
  }
}

</script>

<template>
  
  <hello/>   
  <test1/>
  <Test2/>   //局部注册组件，只能在当前组件使用
  
</template>

<style scoped>

</style>

```
 PascalCase 作为组件名的注册格式
 `  <my-component></my-component>
 `
### 深入props ###
回顾props
props使用的基础
```
//Data.vue中的代码
<script>
import BlogPost from './BlogPost.vue';
export default {
    components: {
        BlogPost,
    },
    data() {
        return {
            posts: [
                 { id: 1, title: "qhgdscb",commentIds:11},
                { id: 2, title: '大海',commentIds:18 },
                {id:3,title:'qwjfbcfewf',commentIds:10},
            ]
        }
    }
   
}
</script>
<template>
   
  <BlogPost v-for="post in posts" :key="post.id" :title="post.title" :likes="post.id" :comment_ids="post.commentIds"/>
</template>
<style>


</style>

```
```
//BlogPost.vue
<script>
export default {
   
    //props:['title']   //字符串数组来声明的prop
      props: {
        title: String,   //以对象的形式来声明prop
        likes:Number
         comment_ids:Array,  //接受一个数组
    }
}
</script>
<template>
      <h1>{{ likes }}</h1>
    <h1>{{ title }}</h1>
     <h1>{{ comment_ids }}</h1>
</template>
<style>


</style>

```
深入props
子组件无法修改父组件传入的props数据。如果需要修改传入的props数据，可以在子组件中使用emit方法触发事件实现。不应该在子组件中去更改一个 prop
### 深入事件 ###
触发与监听事件
```
//直接使用 $emit 方法触发自定义事件
//Test1.vue
<script>
import Mycomponent from '../view/Mycomponent.vue'
export default {
    components: {
        Mycomponent,
    },
    data() {
        return {
             count: 0,
    }
},
    methods:{
        handleSomeEvent(){
            this.count++;
        }
    }
}
</script>
<template>
    <h1>你好，世界！</h1>
   <Mycomponent @someEvent="handleSomeEvent"/>
   {{ count }}
</template>
<style>


</style>
```
```
//Mycomponent.vue
<script>

export default {
  
}
</script>
<template>
    <h1>你好，世界！</h1>
    <button @click="$emit('someEvent')"></button>
</template>
<style>


</style>
```
$emit() 方法在组件实例上也同样以 this.$emit() 的形式可用：
```
//只要将Mycomponent.vue文件修改为如下即可
<script>

export default {
    methods: {
        sumbit() {
        this.$emit('someEvent')
    }
  }
}
</script>
<template>
    <h1>你好，世界！</h1>
    <!-- <button @click="$emit('someEvent')"></button> -->
    <button @click="sumbit"></button>
</template>
<style>


</style>
```
事件参数学习
```
//Test1.vue
<script>
import Mycomponent from '../view/Mycomponent.vue'
export default {
    components: {
        Mycomponent,
    },
    data() {
        return {
             count: 0,
    }
},
    methods:{
        handleSomeEvent(){
            this.count++;
        }
    }
}
</script>
<template>
    <h1>你好，世界！</h1>
   <Mycomponent @someEvent="handleSomeEvent"/>
   {{ count }}
   <Mycomponent @increase-by="(n)=>count+=n"/>
</template>
<style>


</style>
```
```
//Mycomponent.vue
<script>

export default {
    methods: {
        sumbit() {
        this.$emit('someEvent')
    }
  }
}
</script>
<template>
    <h1>你好，世界！</h1>
    <!-- <button @click="$emit('someEvent')"></button> -->
    <!-- <button @click="sumbit"></button> -->
    <button @click="$emit('increaseBy',1)"> Increase by</button>   //给事件传参数

</template>
<style>


</style>
```
还可以写为
```
//Test1.vue
<script>
import Mycomponent from '../view/Mycomponent.vue'
export default {
    components: {
        Mycomponent,
    },
    data() {
        return {
             count: 0,
    }
},
    methods:{
        handleSomeEvent(){
            this.count++;
        },
        increaseCount(n) {
            this.count += n;
        }
    }
}
</script>
<template>
    <h1>你好，世界！</h1>
   <Mycomponent @someEvent="handleSomeEvent"/>
   {{ count }}
   <!-- <Mycomponent @increase-by="(n)=>count+=n"/> -->
   <Mycomponent @increase-by="increaseCount"/>

</template>
<style>


</style>
```
v-model使用在一个组件上
```
//Test1.vue
<script>
import Mycomponent from '../view/Mycomponent.vue'
export default {
    components: {
        Mycomponent,
    },
    data() {
        return {
             count: 0,
             message:"hello",
    }
},
    methods:{
        handleSomeEvent(){
            this.count++;
        },
        increaseCount(n) {
            this.count += n;
        }
    }
}
</script>
<template>
    <h1>你好，世界！</h1>
   <!-- <Mycomponent @someEvent="handleSomeEvent"/> -->
   <!-- {{ count }} -->
   <!-- <Mycomponent @increase-by="(n)=>count+=n"/> -->
   <!-- <Mycomponent @increase-by="increaseCount"/> -->

  <Mycomponent  v-model="message"/>
   //父组件
  
</template>
<style>


</style>
```
```
////Mycomponent.vue
<script>

export default {
    props: ['modelValue'],     //通过父组件传递props  父组件应该写为：modelValue
    emits:['update:modelValue'],   //自定义事件
    methods: {
    //     sumbit() {
    //     this.$emit('someEvent')
    // }
  }
}
</script>
<template>
    <h1>你好，世界！</h1>
    <!-- <button @click="$emit('someEvent')"></button> -->
    <!-- <button @click="sumbit"></button> -->
    <!-- <button @click="$emit('increaseBy',1)"> Increase by</button> -->
    <input :value="modelValue" @input="$emit('update:modelValue',$event.target.value)">   //触发一个自定义的事件，并传了一个参数
  
</template>
<style>


</style>
```
这个例子展示了如何在子组件中使用 v-model 的自定义参数。在父组件中，我们将 title 数据绑定到了 MyComponent 组件的 v-model:title 上。这个 v-model 指令将 title 数据通过 title 属性传递给了 MyComponent 组件，并在 MyComponent 组件内部使用了 title 属性来渲染输入框的值。当用户输入新的值时，MyComponent 组件会触发 update:title 事件，通过这个事件将新的值传递回父组件中。

在子组件中，我们定义了一个名为 title 的属性，它接收了从父组件传递过来的值。同时，我们还定义了一个自定义事件 update:title，当用户输入新的值时，我们通过 $emit 方法触发这个事件，并将新的值传递回父组件中。

这样，我们就实现了在子组件中使用 v-model 的自定义参数。
```
//父组件
<script>
import MyComponent from './MyComponent.vue'

export default {
  components: { MyComponent },
  data() {
    return {
      title: 'v-model argument example'
    }
  }
}
</script>

<template>
  <h1>{{ title }}</h1>
  <MyComponent v-model:title="title" />   //第一个title为title属性是子组件props声明的属性  第二个为绑定的title数据
</template>
```
```
//子组件
<script>
export default {
  props: ['title'],
  emits: ['update:title']
}
</script>

<template>
  <input
    type="text"
    :value="title"
    @input="$emit('update:title', $event.target.value)"
  />
</template>

```


