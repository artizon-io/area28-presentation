## Building Desktop Application with Web Technologies

Presenter: Sam <!-- .element: style="font-size: 30px" -->

![](./assets/one-punch-man.png) <!-- .element width="300px" -->

----

#### Contenders
![](./assets/electron.png) <!-- .element class="fragment" width="200px" -->
![](./assets/tauri.png) <!-- .element class="fragment" width="200px" -->
![](./assets/wails.png) <!-- .element class="fragment" width="250px" -->

---

<!-- .slide: data-background-color="#1f1f1f" -->

### Prerequisites

----

<!-- .slide: data-background-color="#1f1f1f" -->

Definition: **Browser Engine**

> A browser engine (also known as a layout engine or rendering engine) is a core software component of every major web browser. The primary job of a browser engine is to transform HTML documents and other resources of a web page into an interactive visual representation on a user's device. <!-- .element: style="font-size: 32px; line-height: 1.5; word-spacing: 3px;" -->

----

<!-- .slide: data-background-color="#1f1f1f" -->

Definition: **Webview**

> WebView is an Embedded user-agent and typically a web browser UI component that can be embedded in apps to render web pages. <!-- .element: style="font-size: 32px; line-height: 1.5; word-spacing: 3px;" -->

---

<!-- .slide: data-background-color="#1a2641" -->

### Electron

![](./assets/electron.png) <!-- .element width="300px" -->

----

<!-- .slide: data-background-color="#1a2641" -->

![](./assets/electron-users.png) <!-- .element width="1000px" -->

----

<!-- .slide: data-background-color="#1a2641" -->

![](./assets/chromium.png) <!-- .element height="250px" -->
![](./assets/nodejs.png) <!-- .element height="250px" -->

Chromium (webview) + NodeJS (backend) <!-- .element: style="font-size: 38px;" -->

----

<!-- .slide: data-background-color="#1a2641" -->

#### Process Model

![](./assets/electron-process-model.png) <!-- .element width="800px" -->

----

<!-- .slide: data-background-color="#1a2641" -->

#### Main Process

![](./assets/electron-applifecycle.png) <!-- .element width="750px" -->

#### Renderer Process

![](./assets/electron-browserwindow.png) <!-- .element width="750px" -->

But wait, there is a catch... <!-- .element: class="fragment" style="font-size: 28px;" -->

----

<!-- .slide: data-background-color="#1a2641" -->

![](./assets/electron-renderer-nodejs-danger.png)

So how could the renderer be able to leverage the power of Node and Electron? <!-- .element: class="fragment" style="font-size: 28px;" -->

----

<!-- .slide: data-background-color="#1a2641" -->

#### Preload Scripts

Come to the rescue... <!-- .element: style="font-size: 24px;" -->

![](./assets/electron-preload-scripts.png)

```js
// preload.js

const { contextBridge } = require('electron')

contextBridge.exposeInMainWorld('myAPI', {
  desktop: true,
})
```
<!-- .element: class="fragment" style="font-size: 18px;" -->

```js
// renderer.js

console.log(window.myAPI)  // => { desktop: true }

```
<!-- .element: class="fragment" style="font-size: 18px;" -->

----

<!-- .slide: data-background-color="#1a2641" -->

But what if I just want to keep API internal to main process...? <!-- .element: style="font-size: 45px; word-spacing: 3px;" -->

----

<!-- .slide: data-background-color="#1a2641" -->

#### `ipcRenderer` & `ipcMain`

are what you need... <!-- .element: style="font-size: 24px;" -->

```js
// renderer.js

ipcRenderer.send('myChannel', ...args)
```
<!-- .element: class="fragment" style="font-size: 20px; line-height: 1.5;" -->

```js
// main.js

ipcMain.on('myChannel', (...args) => {
  // Do stuff...
})
```
<!-- .element: class="fragment" style="font-size: 20px; line-height: 1.5;" -->

and that's it... <!-- .element: class="fragment" style="font-size: 24px;" -->

---

<!-- .slide: data-background-color="#2f2517" -->

### Tauri

![](./assets/tauri.png) <!-- .element width="300px" -->

----

<!-- .slide: data-background-color="#2f2517" -->

![](./assets/tauri-users.png) <!-- .element: width="800px" -->

Notes:
- [xplorer](https://github.com/kimlimjustin/xplorer)
- [orange](https://github.com/naaive/orange)
- [Time machine inspector](https://github.com/probablykasper/time-machine-inspector)

----

<!-- .slide: data-background-color="#2f2517" -->

#### Tao

Crossplatform application windowing library <!-- .element: style="font-size: 28px;" -->

![](./assets/tao.png)

Tauri actually uses something else for Linux currently... <!-- .element: class="fragment" style="font-size: 24px;" -->

----

<!-- .slide: data-background-color="#2f2517" -->

#### WRY

Webview rendering library

Usng WebKit, WebView2, WebViewGTK, ... <!-- .element: style="font-size: 24px;" -->

![](./assets/wry.png)

Why not use Chromium? <!-- .element: class="fragment" style="font-size: 24px;" -->

----

<!-- .slide: data-background-color="#2f2517" -->

![](./assets/chrome-meme.png)

----

<!-- .slide: data-background-color="#2f2517" -->

Ok... but what about its backend? <!-- .element: style="font-size: 32px;" -->

----

<!-- .slide: data-background-color="#2f2517" -->

#### Rust

![](./assets/rust-game.png)

----

<!-- .slide: data-background-color="#2f2517" -->

Compiled to binary, unlike NodeJS. Expect 1000x performance. <!-- .element: style="font-size: 28px;" -->

![](./assets/rust-kill-js.png)

---

<!-- .slide: data-background-color="#2d1414" -->

### Wails

![](./assets/wales.png) <!-- .element width="600px" -->

---

### Developer Experience

![](./assets/dog-typing.gif) <!-- .element height="280px" -->
![](./assets/cat-typing.gif) <!-- .element height="280px" -->

----

<!-- .slide: data-background-color="#2f2517" -->

#### Tauri

![](./assets/stackoverflow-survey.png) <!-- .element height="250px" -->
![](./assets/rust-loved-language.png) <!-- .element height="250px" -->

----

<!-- .slide: data-background-color="#2d1414" -->

#### Wails

----

<!-- .slide: data-background-color="#1a2641" -->

#### Electron

---

![](./assets/thank-you.gif) <!-- .element height="500px" -->