<template>
  <div>
    <h2>学生姓名:{{ name }}</h2>
    <h2>学生年龄:{{ age }}</h2>
    <button @click="showName">点我提示学生姓名</button>
  </div>
</template>

<script>
    // 组件交互的代码
    // 分别暴露
    export default {
        name: 'Student',
        data(){
            return {
                name:'Jame',
                age:23
            }
        },
        methods: {
            showName(){
                alert(this.name);
            }
        }
    }
    // 统一暴露
</script>