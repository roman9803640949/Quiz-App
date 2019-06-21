<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Fancy Quiz App</b></b-nav-item>
            <b-nav-item disabled>Counter:{{correct}}/{{total}}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
    export default {
        name: "Header",
        props:[
            'total',
            'correct'
        ]
    }
</script>

<style scoped>

</style>