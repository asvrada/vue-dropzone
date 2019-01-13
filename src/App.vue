<template>
    <div id="app">
        <drop-zone v-on:fileUploaded="fileUploaded"></drop-zone>
        <div id="info">File uploaded: <br> {{ info }}</div>
    </div>
</template>

<script>
    import DropZone from './components/DropZone';

    export default {
        name: 'app',
        components: {
            DropZone
        },
        data: function () {
            return {
                info: []
            };
        },
        methods: {
            fileUploaded: function (files) {
                let list = [];

                for (let i = 0; i < files.length; i++) {
                    list.push(files[i].name);
                }

                this.info = list;
            }
        }
    }
</script>

<style lang="scss">
    #app {
        width: 240px;
        height: 150px;

        #info {
            margin: 20px;
        }
    }
</style>
