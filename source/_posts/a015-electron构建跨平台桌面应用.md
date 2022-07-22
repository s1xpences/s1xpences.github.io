---
title: electron构建跨平台桌面应用
categories:
  - 前端学习
tags:
  - electron
date: 2022-07-22 16:23:34
index_img: https://img0.baidu.com/it/u=347447089,4194412610&fm=253&fmt=auto&app=138&f=JPEG?w=640&h=411
---

### electron-vue应用自动更新
1. 安装 npm i electron-updater --save
npm包版本：

```json
"vue": "^2.5.16",
"vue-electron": "^1.0.6",
"electron-updater": "^4.3.0",
```

2. package.json build下添加

```json
    "nsis": {
      "perMachine": false,
      "allowToChangeInstallationDirectory": false,
      "runAfterFinish": true
    },
    "publish": [
      {
        "provider": "generic",
        "url": "https://csfile.ossxrcloud.net/moon"
      }
    ],
```

3. util/update.js

```javascript
import { autoUpdater } from 'electron-updater'
import { ipcMain } from 'electron'
const fs = require('fs')
let mainWindow = null;

export function updateHandle(window, feedUrl) {
    mainWindow = window;
    let message = {
        error: '检查更新出错',
        checking: '正在检查更新……',
        updateAva: '检测到新版本，正在下载……',
        updateNotAva: '现在使用的就是最新版本，不用更新',
    };
    // autoUpdater.autoDownload = false
    //设置更新包的地址
    autoUpdater.setFeedURL(feedUrl);
    //监听升级失败事件
    autoUpdater.on('error', function (error) {
        sendUpdateMessage({
            cmd: 'error',
            message: error
        })
    });
    //监听开始检测更新事件
    autoUpdater.on('checking-for-update', function (message) {
        sendUpdateMessage({
            cmd: 'checking-for-update',
            message: message
        })
    });
    //监听发现可用更新事件
    autoUpdater.on('update-available', function (message) {
        //先删除当前文件夹及其文件
        deleteall('C:/Users/Administrator/AppData/Local/moonbox-updater/pending')
        sendUpdateMessage({
            cmd: 'update-available',
            message: message
        })
    });

    //监听没有可用更新事件
    autoUpdater.on('update-not-available', function (message) {
        sendUpdateMessage({
            cmd: 'update-not-available',
            message: message
        })
    });

    // 更新下载进度事件
    autoUpdater.on('download-progress', function (progressObj) {
        sendUpdateMessage({
            cmd: 'download-progress',
            message: progressObj
        })
        mainWindow.webContents.send('downloadProgress', progressObj)
    });

    //监听下载完成事件
    autoUpdater.on('update-downloaded', function (event, releaseNotes, releaseName, releaseDate, updateUrl) {
        sendUpdateMessage({
            cmd: 'update-downloaded',
            message: {
                releaseNotes,
                releaseName,
                releaseDate,
                updateUrl
            }
        })
        //退出并安装更新包
        autoUpdater.quitAndInstall();
    });

    ipcMain.on("checkForUpdate", (e, arg) => {
        //组件created执行自动更新检查
        autoUpdater.checkForUpdates();
    })

    ipcMain.on('downloadUpdate', () => {
        //点击更新按钮
        autoUpdater.downloadUpdate()
    })
}

//给渲染进程发送消息
function sendUpdateMessage(text) {
    mainWindow.webContents.send('message', text)
}

//更新时删除文件夹及其文件
function deleteall(path) {
    var files = [];
    if (fs.existsSync(path)) {
        files = fs.readdirSync(path);
        files.forEach(function (file, index) {
            var curPath = path + "/" + file;
            if (fs.statSync(curPath).isDirectory()) { // recurse
                deleteall(curPath);
            } else { // delete file
                fs.unlinkSync(curPath);
            }
        });
        fs.rmdirSync(path);
    }
};
```

4. main/index.js 主进程

```javascript
import {
    updateHandle
} from '../renderer/utils/update'

//检测版本更新
let feedUrl = "https://csfile.ossxrcloud.net/moon";
updateHandle(mainWindow, feedUrl);
```

5. .vue文件

```javascript
<template>
    <div id="homeview">
     <div class="update-model" v-if="updateModal.updateShow">
      <div class="mask">
        <div class="wrap">
          <div class="title">
            更新中...({{ updateModal.downloadPercent + "%" }})
          </div>
          <div class="update-progress">
            <Progress
              :percent="updateModal.downloadPercent"
              :stroke-width="8"
              :stroke-color="['#00C9FF', '#2D6FFF']"
              hide-info
            />
          </div>
        </div>
      </div>
     </div>
    </div>
</template>

<script>
import { shell, ipcRenderer } from "electron";
data() {
    return {
      //自动更新
      updateModal: {
        downloadPercent: 0,
        updateShow: false,
      },
    }
},
created() { 
    //自动更新
    this.listenUpdate();
    ipcRenderer.send("checkForUpdate");
},
methods: {
    //自动更新
    listenUpdate() {
      ipcRenderer.on("message", (event, arg) => {
        // console.log(arg);
        if ("update-available" == arg.cmd) {
          this.updateModal.updateShow = true;
        } else if ("download-progress" == arg.cmd) {
          this.updateModal.downloadPercent = parseInt(arg.message.percent);
        } else if ("error" == arg.cmd) {
          // this.updateModal.updateErrorShow = true;
        }
      });
    },
}
</script>
```

### 应用打包踩坑
待更新...

---
官方文档：[electronjs](https://www.electronjs.org/)