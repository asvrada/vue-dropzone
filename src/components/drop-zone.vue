<template>
    <!--emit fileUploaded event open file received-->
    <div id="drop-zone"
         v-on:dragenter.prevent.stop="handleDragEnter"
         v-on:dragleave.prevent.stop="handleDragLeave"
         v-on:dragover.prevent.stop="handleDragOver"
         v-on:drop.prevent.stop="handleDrop"
         v-on:click="promptSelectFile"

         v-bind:class="{promptDropClass:promptDrop, loadedClass:!!fileLoaded}"
         v-bind:style="styleObjectApp"
    >

        <div id="div-prompt-text">
            <component id="area-icon" v-bind:is="currentComponent" v-bind:style="styleObjectSVG"></component>
        </div>

        <form v-show="false" class="form-file-upload">
            <input type="file" id="elem-file" v-on:change="handleForm">
            <label ref="btn_form" class="button" for="elem-file">Select subtitle file</label>
        </form>
    </div>
</template>

<script>
    import IconInit from "./icon-init";
    import IconSelected from "./icon-selected";

    export default {
        name: "drop-zone",
        components: {
            IconInit,
            IconSelected
        },
        data: function () {
            return {
                fileLoaded: null,
                promptDrop: false,
                style: {
                    color: {
                        init: "#303F9F",
                        drop: "#FFA000",
                        loaded: "#689F38"
                    },
                    ratio: 1.6,
                    height: 140,
                    svgSizeMultiplier: 0.5,
                    borderThickness: 10
                }
            }
        },
        computed: {
            currentState: function () {
                if (this.promptDrop) {
                    return "drop";
                }

                if (this.fileLoaded !== null) {
                    return "loaded";
                }

                return "init";
            },
            styleObjectApp: function () {
                const style = this.style;

                let state = this.currentState;

                return {
                    height: `${style.height}px`,
                    width: `${style.height * style.ratio}px`,
                    "background-color": `${style.color[state]}`
                }
            },
            styleObjectSVG: function () {
                const style = this.style;

                let outerHeight = style.height - (this.promptDrop ? 2 * style.borderThickness : 0);
                let thisHeight = style.height * style.svgSizeMultiplier;
                let padding = (outerHeight - thisHeight) / 2;
                return {
                    width: `${thisHeight}px`,
                    "padding-top": `${padding}px`,
                    "padding-bottom": `${padding}px`,
                }
            },
            currentComponent: function () {
                return {
                    "init": "IconInit",
                    "loaded": "IconSelected",
                    "drop": "IconSelected"
                }[this.currentState];
            }
        },
        methods: {
            handleDragEnter() {
                this.promptDrop = true;
            },
            handleDragLeave() {
                this.promptDrop = false;
            },
            handleDragOver(e) {
                // we don't need this ...
            },
            handleDrop(e) {
                let file = null;

                try {
                    let dt = e.dataTransfer;
                    file = dt.files[0];
                } catch (e) {
                    alert("There was a problem processing your file, please try again");
                    console.log("Error drag & drop file", e)
                }

                this.callbackFileHandler(file);

            },
            handleForm(e) {
                let file = null;

                try {
                    let dt = e.target;
                    file = dt.files[0];
                } catch (e) {
                    alert("There was a problem processing your file, please try again");
                    console.log("Error input file", e)
                }

                this.callbackFileHandler(file);
            },
            /**
             * Calls the file handler passed in from outside
             * @param file
             */
            callbackFileHandler(file) {
                this.fileLoaded = file;
                this.promptDrop = false;

                this.$emit('fileUploaded', file);
            },
            /**
             * User clicks the upload area, prompt user to select files from system
             */
            promptSelectFile() {
                this.$refs.btn_form.click();
            }
        }
    }
</script>

<style lang="scss" scoped>
    #div-prompt-text {
    }

    #area-icon {
        margin: 0 auto;
    }

    #drop-zone {
        box-sizing: border-box;

        border-radius: 20px;

        cursor: pointer;
    }

    /* style after dragEnter */
    .promptDropClass {
        border: 10px dashed #707070;
    }

    .loadedClass {
    }
</style>