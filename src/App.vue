<template>
    <div id="app">
        <drop-zone></drop-zone>
    </div>
</template>

<script>
    import DropZone from './components/DropZone';

    export default {
        name: 'app',
        components: {
            DropZone
        }
    }
</script>

<style>
    #app {
        width: 240px;
        height: 150px;
    }
</style>
