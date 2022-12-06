<template>
  <div>
    <h1>
      <a :herf="link">{{title}}</a>
    </h1>
    <article>
      <div>
        {{content}}
      </div>
      <!-- ref定义的数据，在模版template中访问时不需要访问value属性，
        value会被自动拆解，只有在script里才需要 -->
      <p>View Count:{{viewCount}}</p>
      <footer v-if="(content.length > 10)">
      <button>read original</button>
      </footer>
      
    </article>
    <!-- slot进行占位，可以在父组件进行替换 -->
    <slot></slot>
  </div>
</template>

<!-- 让组件接收属性，通过父组件进行传递 -->
<script setup>
import { onMounted, ref } from 'vue';

// 定义接收的属性名
defineProps(["title","content","link"])

// 定义阅读量，初始值为0
const viewCount = ref(0);

// 在组件挂在之后执行一些代码，我们可以使用下面这个生命周期
onMounted( () => {
  setTimeout(()=>{ // 传递一个回调函数，在里面可以请求一个远程函数
    viewCount.value = 10000; 
  },1000) //一秒之后执行
})

</script>
