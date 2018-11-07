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
                <div class="pane text-center">
                    <textarea v-model="content">

                    </textarea>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import {ipcRenderer, remote} from 'electron';

    let currentFile = null;
    export default {
        name: 'landing-page',
        data() {
            return {
                content: ""
            }
        },
        methods: {
            open() {
                remote.getCurrentWindow().webContents.send('action', 'open');
            },
            readText(file) {
                const fs = require('fs');
                return fs.readFileSync(file, 'utf8');
            },
            //保存当前文档
            saveCurrentDoc() {
                if (!currentFile) {
                    const file = remote.dialog.showSaveDialog(remote.getCurrentWindow(), {
                        filters: [
                            {name: "Text Files", extensions: ['txt', 'js', 'html', 'md']},
                            {name: 'All Files', extensions: ['*']}]
                    });
                    if (file) currentFile = file;
                }
                if (currentFile) {
                    const txtSave = this.content;
                    this.saveText(txtSave, currentFile);
                    // document.title = "Notepad - " + currentFile;
                }
            },
            //保存文本内容到文件
            saveText(text, file) {
                const fs = require('fs');
                fs.writeFileSync(file, text);
            }
        },
        mounted() {
            //监听与主进程的通信
            ipcRenderer.on('action', (event, arg) => {
                console.log(arg)
                switch (arg) {
                    case 'open': //打开文件
                        const files = remote.dialog.showOpenDialog(remote.getCurrentWindow(), {
                            filters: [
                                {name: "Text Files", extensions: ['txt', 'js', 'html', 'md']},
                                {name: 'All Files', extensions: ['*']}],
                            properties: ['openFile']
                        });
                        if (files) {
                            currentFile = files[0];
                            const txtRead = this.readText(currentFile);
                            this.content = txtRead;
                        }
                        break;
                    case 'save': //保存文件
                        this.saveCurrentDoc();
                        break;
                    case 'close': //保存文件
                        this.content = "";
                        currentFile = null;
                        break;
                }
            });
        }
    }
</script>

<style>

    button {
        padding-top: 200px;
    }

    textarea {
        width: 100%;
        height: 99.4%;
        border: none;
        outline: none
    }
</style>
