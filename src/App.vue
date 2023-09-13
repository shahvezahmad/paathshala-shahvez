<template>
    <Form/>
    <Card/>
</template>

<script>
    import Form from './components/Form.vue';
    import Card from './components/Card.vue';
    export default {
        name: 'App',
        components: { Form, Card },
    };
</script>

<style scoped>

    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
    body {
        margin: 0;
        background: #eee;
    }
    
</style>
