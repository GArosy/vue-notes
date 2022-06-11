<template>
    <div class="demo">
        <h2>学校名称:{{ name }}</h2>
        <h2>学校地址:{{ address }}</h2>
        <button @click="showName">点我提示学校名</button>
    </div>
</template>

<style>
    .demo {
        background: skyblue;
    }
</style>

<script>
    export default {
        name: 'School',
        data(){
            return {
                name: 'CUMT',
                address: 'Xuzhou'
            }
        },
        methods: {
            showName(){
                console.log(this);
                alert(this.name)
            }
        }
    }
</script>