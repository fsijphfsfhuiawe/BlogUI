<script setup>
// 可以在这里定义变量
import { computed, ref } from 'vue';

// 定义数组
const blogs = ref([
  {
    id:1,
    title:"Personal Blog",
    content:"This is a personal blog",
    link:"/blog-main-page",
  },
  {
    id:2,
    title:"Personal Blog",
    content:"This is a personal blog",
    link:"/blog-main-page",
  },
  {
  id:3,
  title:"Personal Blog",
  content:"This is a personal blog",
  link:"/blog-main-page",
}
])


// const blog = ref({
//   title:"Personal Blog",
//   content:"This is a personal blog",
//   link:"/blog-main-page"
// })



const showTotal = ref(true);

// 使用ref定义的数据，必须用value才能访问到真实数据
const total = computed(()=>blogs.value.length);

// js的函数
function toggleTotal(){
  showTotal.value = !showTotal.value;
}

const initialBlogForm = {
  title:"",
  content:"",
  link:"",
}

const blogForm = ref({...initialBlogForm});

function addBlog(){
  blogs.value.push({
    id: blogs.value.length + 1,
    ...blogForm.value,
  });
  // 提交完成后刷新提交面板(重新设置为初始值)
  blogForm.value = {...initialBlogForm};
}

</script>

<!-- 在main.css里可以修改样式 -->
<template>
  <!-- 在template对script里的变量或常量进行引用 -->
  <div v-for="blog in blogs" :key="blog.id">
    <h1>
      <a :herf="blog.link">{{blog.title}}</a>
    </h1>
    <article>
      <div>
        {{blog.content}}
      </div>
      <footer v-if="(blog.content.length > 10)">
      <button>read original</button>
      </footer>
      <!-- 处理事件 -->
      <h3 v-if="showTotal">total={{total}}</h3>
      <button @click="toggleTotal">{{showTotal ? "hide":"show"}}</button>
    </article>
  </div>

  <form @submit.prevent="addBlog">
    <label for="blogTitle">blog title</label>
    <input type="text" id="blogTitle" v-model="blogForm.title"/>
    <label for="content" >content</label>
    <textarea id="content" cols="30" rows="10" v-model="blogForm.content"></textarea>
    <label for="link">link</label>
    <input type="text" id="link" v-model="blogForm.link"/>
    <button type="submit">submit</button>
  </form>
</template>

<style scoped>
form{
  display: grid;
  margin-top: 2em;
}
</style>
