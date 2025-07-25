# 全端开发方案

1. RN-CLI + EXPO + electron（electronforge构建器）方案

```
 npx ignite-cli@latest new pizza-app
 构建RN-CLI + EXPO的RN项目
 cd pizza-app && pod install
 npm run ios
 npm run android
 npm run web

 打包web
 npm run build:web
 测试web
 npm run serve:web

 集成electronforge
 npx create-electron-app@latest pizza-pc-app --template=vite

  copy dist -> pizza-pc-app


  cd pizza-pc-app
  npm run start
  ================
  cd pizza-app

  .gitignore 文件

  add 《pizza-pc-app/_expo/*   pizza-pc-app/assets/*》

  fix bug   文件
        /pizza-app/app/screens/DemoShowroomScreen/DemoShowroomScreen.tsx
        if(findItemIndex === -1){
          findItemIndex = 0
        }

  app.json  文件
          "experiments": {
            "tsconfigPaths": true,
            "baseUrl": ""
          }


  mkdir public


  mkfile index.html


  index.html 文件=
`

<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="utf-8" />
<meta httpEquiv="X-UA-Compatible" content="IE=edge" />
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />
<meta name="theme-color" content="#000000" />
<link rel="manifest" href="/manifest.json" />
<link rel="stylesheet" type="text/css" href="https://cdn.bootcdn.net/ajax/libs/normalize/8.0.1/normalize.min.css" />
<title>PizzaApp</title>
<style>
  html,
  body {
    height: 100%;
  }

  body {
    overflow: hidden;
  }

  #root {
    display: flex;
    height: 100%;
    flex: 1;
  }
</style>
</head>

<body>
<noscript>
  You need to enable JavaScript to run this app.
</noscript>
<div id="root"></div>
<script type="text/javascript" charset="utf-8">
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/sw.js').then(registration => {
      console.log('Service Worker registered with scope:', registration.scope);
    }).catch(error => {
      console.error('Service Worker registration failed:', error);
    });
  });
}
</script>
<script type="module" src="/src/renderer.js" async defer></script>
</body>

</html>

`

metro.config.js 文件

config.transformer.getTransformOptions = async () => ({
transform: {
  inlineRequires: true,
  // experimentalImportSupport: true,
},
compress: {
  drop_console: true,
},
})


/pizza-app/pizza-pc-app/manifest.json 文件

{
"short_name": "pizza App",
"name": "pizza Sample",
"icons": [
  {
    "src": "favicon.ico",
    "sizes": "64x64 32x32 24x24 16x16",
    "type": "image/x-icon"
  },
  {
    "src": "logo192.png",
    "type": "image/png",
    "sizes": "192x192"
  },
  {
    "src": "logo512.png",
    "type": "image/png",
    "sizes": "512x512"
  }
],
"start_url": ".",
"display": "standalone",
"theme_color": "#000000",
"background_color": "#ffffff"
}

/pizza-app/pizza-pc-app/src/preload.js 文件

const { contextBridge } = require('electron')

contextBridge.exposeInMainWorld('versions', {
node: () => process.versions.node,
chrome: () => process.versions.chrome,
electron: () => process.versions.electron
// 除函数之外，我们也可以暴露变量
})

/pizza-app/pizza-pc-app/src/renderer.js 文件

import './index.css';
const information = `本应用正在使用 Chrome (v${versions.chrome()}), Node.js (v${versions.node()}), 和 Electron (v${versions.electron()})`
console.log('👋 This message is being logged by "renderer.js", included via Vite');
console.info(`👋 ${information}`);



```
