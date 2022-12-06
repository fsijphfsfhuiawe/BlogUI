<script setup>
import { onMounted, ref, toRefs, watch } from 'vue';
import * as echarts from "echarts";

// 在Vue3中template ref和响应式状态的ref合为了一体，
// 所以我们定义一个ref来保存dom元素
const container = ref(null);

// 由于不能在watch中直接访问onMounted里的chart，
// 因此我们也把chart保存在ref中，这样就可以使用了
const chart = ref(null);

// 让chart接收一个options属性用于接收图表page
const props = defineProps({
  options:{
    required: true, //是一个必须的属性
    type: Object, //类型为对象
    // default: { }, //默认值是空的对象
  }
  //当options变化时，图表不会刷新
})

// ref中的元素在dom加载完成之后才会有值，
// 所以我们在onMounted生命周期中获取它
onMounted(()=>{
  chart.value = echarts.init(container.value, "dark");
  chart.value.setOption(props.options); //初始化完成之后将page传递给chart实例
})

const {options} = toRefs(props); // 把options转化为ref传递给watch
watch(
  options, //watch的第一个参数可以是一个ref
  (newOptions) => { //第二个参数是一个处理函数，它接收两个参数：
    chart.value.setOption(newOptions); //监听的属性新变化的值和之前的值
  },
  {deep: true} //第三个参数为是否深度对比
)
</script>

<template>
  <div class="container" ref="container"></div>
</template>

<style scoped>
.container{
  width: 100%;
  height: 100%;
}
</style>
