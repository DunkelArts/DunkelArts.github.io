---
title: "Destiny Inventory Manager"
layout: "post"
#url: "/archives/"
summary: Destiny Inventory Manager packaged with Electron.js to use as a desktop application.
---

Destiny Inventory Manager packaged with Electron.js to use as a desktop application.

Electron is a framework that enables you to create desktop applications with JavaScript, HTML, and CSS. These applications can then be packaged to run directly as an .exe



```js
const { BrowserWindow, app, Menu } = require('electron')

function createwindow() {

const win = new BrowserWindow({
    title: "Destiny Inventory Manager"
})

win.loadURL('https://app.destinyitemmanager.com/')

win.maximize()

var menu = Menu.buildFromTemplate([
    {
      label: 'File',
      submenu: [
        {
          label:'Exit',
          accelerator: 'CmdOrCtrl+Q',
          click() {
            app.quit();
          }
        }
      ],
    },
    {
      label: 'Update Contents',
      submenu: [
        {
          label:'Reload Page',
          accelerator: 'F5',
          click() {
            win.reload()
          }
        },
        {
          label:'Force Reload',
          accelerator: 'CmdOrCtrl+F5',
          click() {
            win.webContents.reloadIgnoringCache()
          }
        },
        {
            label:'Developer Console',
            accelerator: 'F12',
            click() {
              win.webContents.openDevTools()
            }
        }, 
      ],
    },
    {
      label: 'Fireteam',
      submenu: [
        {
          label:'Search Fireteam',
          click() {
            win.loadURL('https://www.bungie.net/en/ClanV2/FireteamSearch/')
          }
        }
      ],
    },
    {
    label: 'Item Manager',
    submenu: [
      {
        label:'Open DIM',
        click() {
          win.loadURL('https://app.destinyitemmanager.com/')
        }
      }
    ],
  }
  ])
  Menu.setApplicationMenu(menu);
}

app.whenReady().then(createwindow)

app.on('window-all-closed', () => {
    if(process.platform !== 'darwin') {
        app.quit()
    }
})

app.on('activate', () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow()
    }
})
```

## Nodejs Package

Built & Run with Node.js

```json
{
  "name": "dim",
  "version": "1.0.0",
  "description": "DIM",
  "main": "main.js",
  "scripts": {
    "start": "electron .",
    "build": "electron-packager . dim --platform win32 --arch x64 --icon=resources/dim.ico --out dist/"
  },
  "author": "DunkelArts",
  "license": "ISC",
  "devDependencies": {
    "electron": "^9.1.2",
    "electron-packager": "^15.0.0"
  }
}
```