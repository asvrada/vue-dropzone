<template>
    <div id="app">
        <drop-zone></drop-zone>
    </div>
</template>

<script>
    import DropZone from './components/drop-zone';

    export default {
        name: 'app',
        components: {
            DropZone
        }
    }
</script>

<style>
    #app {
    }
</style>
