<template>
    <div class="window">
        <div class="window-content">
            <div class="pane-group">
                <div class="pane-sm sidebar">
                    <nav>
                        <h5 class="nav-group-title">工具栏</h5>
                        <span class="nav-group-item" @click="open()">
                            <span class="icon icon-folder"></span>
                            打开
                      </span>

                    </nav>
                </div>
                <div class="pane text-center">{{content}}</div>
            </div>
        </div>
    </div>
</template>

<script>
    import {ipcRenderer, remote} from 'electron';

    export default {
        name: 'landing-page',
        data() {
            return {
                content: ""
            }
        },
        methods: {
            open() {
                //监听与主进程的通信
                ipcRenderer.on('action', (event, arg) => {
                    switch (arg) {
                        case 'open': //打开文件

                            const files = remote.dialog.showOpenDialog(remote.getCurrentWindow(), {
                                filters: [
                                    {name: "Text Files", extensions: ['txt', 'js', 'html', 'md']},
                                    {name: 'All Files', extensions: ['*']}],
                                properties: ['openFile']
                            });
                            console.log(files)
                            if (files) {
                                const txtRead = this.readText(files[0]);
                                this.content = txtRead;
                            }
                            break;
                    }
                });
            },
            readText(file) {
                const fs = require('fs');
                return fs.readFileSync(file, 'utf8');
            }
        },
        mounted() {
            this.open();
        }
    }
</script>

<style>

    button {
        padding-top: 200px;
    }
</style>
