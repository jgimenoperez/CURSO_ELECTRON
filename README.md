# CURSO_ELECTRON

## Guia electron
    https://medium.com/@xagustin93/a%C3%B1adiendo-react-a-una-aplicaci%C3%B3n-de-electron-ab3df35f48fd


    Añadir electron a nuestra aplicación de Ract JS
    yarn electron electron-builder --dev
    yarn add wait-on concurrently --dev
    yarn add electron-is-dev


    Crea un nuevo archivo en, /public/electron.js, con el siguiente contenido.

        const electron = require('electron');
        const app = electron.app;
        const BrowserWindow = electron.BrowserWindow;

        const path = require('path');
        const isDev = require('electron-is-dev');

        let mainWindow;

        function createWindow() {
        mainWindow = new BrowserWindow({width: 900, height: 680});
        mainWindow.loadURL(isDev ? 'http://localhost:3000' : `file://${path.join(__dirname, '../build/index.html')}`);
        if (isDev) {
            // Open the DevTools.
            //BrowserWindow.addDevToolsExtension('<location to your react chrome extension>');
            mainWindow.webContents.openDevTools();
        }
        mainWindow.on('closed', () => mainWindow = null);
        }

        app.on('ready', createWindow);

        app.on('window-all-closed', () => {
        if (process.platform !== 'darwin') {
            app.quit();
        }
        });

        app.on('activate', () => {
        if (mainWindow === null) {
            createWindow();
        }
        });

    Agregamos el siguiente comando a la etiqueta de scripts package.json.
        "electron-dev": "concurrently \"BROWSER=none yarn start\" \"wait-on http://localhost:3000 && electron .\""
    
    Ahora agregamos la siguiente etiqueta main a package.json.
        "main": "src/electron.js",

    Lanzar con 
        yarn electron-dev
