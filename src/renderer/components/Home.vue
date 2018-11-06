<template>
    <div id="wrapper" class="bg-all">
        <h2>Hello world</h2>
    </div>
</template>

<script>

    export default {
        name: 'landing-page',
        methods: {
            open(link) {
                this.$electron.shell.openExternal(link)
            }
        }
    }
</script>

<style>
    .bg-all{
        background: aqua;
    }
    h2{

    }
</style>
