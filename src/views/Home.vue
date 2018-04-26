<template>
    <my-page title="上传" :page="page">
        <div ref="dropArea" class="drop-box"
                @dragenter="handleDragEnter($event)"
                @dragleave="handleDragLeave($event)"
                @drop="handleDrop($event)"
                @dragover='allowDrop($event)'>
            <ui-raised-button class="file-select-btn" label="上传文件" primary>
                <!-- <input type="file" class="ui-file-button" accept="image/*" @change="fileChange($event)"> -->
                <input type="file" class="ui-file-button" @change="fileChange($event)">
            </ui-raised-button>
            <div class="text">把文件拖到这里，快速上传</div>
        </div>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
                page: {
                    menu: [
                    ]
                }
            }
        },
        mounted() {
            this.init()
        },
        methods: {
            init() {
                this.initWebIntents()
            },
            initWebIntents() {
            },
            handleDragEnter(e) {
                console.log('handleDragEnter')
                e.preventDefault()
            },
            handleDragLeave(e) {
                console.log('handleDragLeave')
                e.preventDefault()
            },
            dealFile(file) {
                let reader = new FileReader()
                reader.onload = e => {
                    this.data = e.target.result
                    if (window.intent && !this.hasAddMenu) {
                        this.hasAddMenu = true
                        this.page.menu.push({
                            type: 'icon',
                            icon: 'check',
                            click: this.finish,
                            title: '完成'
                        })
                    }
                }
                if (window.intent) {
                    if (window.intent.type.includes('image')) {
                        reader.readAsDataURL(file)
                    } else {
                        reader.readAsText(file)
                    }
                }
            },
            handleDrop(e) {
                console.log('drop')
                console.log(e)
                let file = e.dataTransfer.files[0]
                this.dealFile(file)
                e.preventDefault()
            },
            allowDrop(e) {
                e.preventDefault()
            },
            fileChange(e) {
                let files = e.target.files
                if (!files.length) {
                    return
                }
                this.dealFile(files[0])
            },
            finish() {
                window.intent.postResult(this.data)
                setTimeout(() => {
                    let owner = window.opener || window.parent
                    owner.window.close()
                }, 100)
            }
        }
    }
</script>

<style lang="scss" scoped>
    .drop-box{
        max-width: 480px;
        margin: 80px auto;
        height: 180px;
        padding-top: 56px;
        border: 2px dashed #999;
        text-align: center;
        .text {
            margin-top: 24px;
        }
    }
</style>
