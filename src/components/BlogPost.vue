<template>
  <div>
    <h1>
      <!-- 添加@click来处理原生点击事件，且prevent自动的跳转，
        $emit调用'titleClick'事件，并传递title作为参数
        在点击的时候，父组件监听事件并能够获取title属性值 -->
      <a :herf="link" @click.prevent="$emit('titleClick', title)">{{title}}</a>
    </h1>
    <article>
      <div>
        {{content.slice(0,100)}}
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
// 定义触发事件,titleClick为设置的名字
defineEmits(["titleClick"])

// 定义阅读量，初始值为0
const viewCount = ref(0);

// 在组件挂在之后执行一些代码，我们可以使用下面这个生命周期
onMounted( () => {
  setTimeout(()=>{ // 传递一个回调函数，在里面可以请求一个远程函数
    viewCount.value = 10000; 
  },1000) //一秒之后执行
})

</script>
