<template>
    <my-page title="上传">
        <ui-raised-button class="file-select-btn" label="上传文件" primary>
            <!-- <input type="file" class="ui-file-button" accept="image/*" @change="fileChange($event)"> -->
            <input type="file" class="ui-file-button" @change="fileChange($event)">
        </ui-raised-button>
    </my-page>
</template>

<script>
    export default {
        data () {
            return {
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'help',
                            to: '/help'
                        }
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
            fileChange(e) {
                let files = e.target.files
                if (files.length > 0) {
                    let reader = new FileReader()
                    reader.onload = e => {
                        this.data = e.target.result
                    }
                    reader.readAsDataURL(files[0])
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

<style scoped>
</style>
