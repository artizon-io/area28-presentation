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

> A browser engine (also known as a layout engine or rendering engine) is a core software component of every major web browser. The primary job of a browser engine is to transform HTML documents and other resources of a web page into an interactive visual representation on a user's device. <!-- .element: style="font-size: 30px; line-height: 1.3; word-spacing: 3px;" -->

![](./assets/browser-engines.png)

----

![](./assets/browser-engines-history.png) <!-- .element height="600px" -->

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

Chromium (webview) + NodeJS (backend) <!-- .element: style="font-size: 35px;" -->

----

<!-- .slide: data-background-color="#1a2641" -->

![](./assets/electron-meme.png) <!-- .element height="600px" -->

----

<!-- .slide: data-background-color="#1a2641" -->

![](./assets/chrome-meme.png)

----

<!-- .slide: data-background-color="#1a2641" -->

#### Process Model

![](./assets/electron-process-model.png) <!-- .element width="800px" -->

----

<!-- .slide: data-background-color="#1a2641" -->

##### Main Process

![](./assets/electron-applifecycle.png) <!-- .element width="750px" -->

##### Renderer Process

![](./assets/electron-browserwindow.png) <!-- .element width="750px" -->

But wait, there is a catch... <!-- .element: class="fragment" style="font-size: 26px;" -->

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

But what if I just want to keep API internal to the main process...? <!-- .element: style="font-size: 32px; word-spacing: 3px;" -->

----

<!-- .slide: data-background-color="#1a2641" -->

#### `ipcRenderer` & `ipcMain`

have you covered... <!-- .element: style="font-size: 24px;" -->

```js
// renderer.js

ipcRenderer.send('myChannel', ...args)
```
<!-- .element: class="fragment" style="font-size: 20px; line-height: 1.5;" -->

```js
// main.js

ipcMain.on('myChannel', (...args) => {
  // Some logic that you don't want to be exposed to renderers...
})
```
<!-- .element: class="fragment" style="font-size: 20px; line-height: 1.5;" -->

----

<!-- .slide: data-background-color="#1a2641" -->

#### Build Lifecycle

![](./assets/electron-build-cycle.png) <!-- .element height="500px" -->

and that's Electron in a nutshell... <!-- .element: class="fragment" style="font-size: 24px;" -->

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

----

<!-- .slide: data-background-color="#2f2517" -->

#### WRY

Webview rendering library <!-- .element: style="font-size: 28px;" -->

Behind the scene uses system webviews (via dynamic linking) <!-- .element: style="font-size: 24px; color: #acacac" -->

+ Lightweight + small bundle size <!-- .element: class="fragment" style="font-size: 24px; color: #50d96d;" -->
- Need to cater all different rendering environments... <!-- .element: class="fragment" style="font-size: 24px; color: #d95059;" -->

![](./assets/wry.png)

----

<!-- .slide: data-background-color="#2f2517" -->

We talked about *Tau (TAO)* <!-- .element: style="font-size: 32px; color: #13c4f0;" -->
, and *ri (WRY)* <!-- .element: style="font-size: 32px; color: #f0a313;" -->
<!-- .element: style="font-size: 32px;" -->

But what about its backend? <!-- .element: class="fragment" style="font-size: 32px;" -->

----

<!-- .slide: data-background-color="#2f2517" -->

#### Rust

![](./assets/rust-game.png) <!-- .element: width="800px" -->

----

<!-- .slide: data-background-color="#2f2517" -->

Compile directly to binary, without needing a runtime (unlike NodeJS) <!-- .element: style="font-size: 26px;" -->

Expect up to **10x**<!-- .element: style="font-size: 40px; color: #13c4f0;" --> performance (speed, memory, size)
<!-- .element: style="font-size: 26px;" -->

![](./assets/rust-kill-js.png) <!-- .element: width="800px" -->

----

<!-- .slide: data-background-color="#2f2517" -->

#### Process Model

![](./assets/tauri-process-model.png) <!-- .element: width="800px" -->

Identical to Electron's and Chrome's... <!-- .element: class="fragment" style="font-size: 24px;" -->

----

<!-- .slide: data-background-color="#2f2517" -->

#### IPC - Event

Core <-> Webview (both-way) <!-- .element: style="font-size: 28px;" -->

![](./assets/tauri-ipc-event.png) <!-- .element: width="800px" -->

----

<!-- .slide: data-background-color="#2f2517" -->

Example of IPC Event <!-- .element: style="font-size: 28px;" -->

```ts[5-7|9-11]
// Webview

import { emit, listen } from '@tauri-apps/api/event'

const unlisten = await listen('some-event', event => {
  // Some complex logic
})

emit('some-event', {
  // Some data payload
})
```
<!-- .element: class="fragment" style="font-size: 18px; line-height: 1.3;" -->

```rust[3-5|7-9]
// Core

let id = app.listen_global("some-event", |event| {
  // Some complex logic
});

app.emit_all("some-event", Payload {
  // Some data payload
}).unwrap();
```
<!-- .element: class="fragment" style="font-size: 18px; line-height: 1.3;" -->

----

<!-- .slide: data-background-color="#2f2517" -->

#### IPC - Command

Webview -> Core <!-- .element: style="font-size: 28px;" -->

![](./assets/tauri-ipc-command.png) <!-- .element: width="800px" -->

----

<!-- .slide: data-background-color="#2f2517" -->

Example of IPC Command <!-- .element: style="font-size: 28px;" -->

```rust
// Inside core

#[tauri::command]
fn some_command() {
  // Some extremely complex logic
}
```
<!-- .element: class="fragment" style="font-size: 20px; line-height: 1.5;" -->

```ts
// Inside webview

import { invoke } from "@tauri-apps/api";

await invoke("some_command");
```
<!-- .element: class="fragment" style="font-size: 20px; line-height: 1.5;" -->

----

<!-- .slide: data-background-color="#2f2517" -->

Sidetrack: difference between *Python decorator*<!-- .element: style="color: #13c4f0;" --> and *Rust procedural macro*<!-- .element: style="color: #f05513;" -->...
<!-- .element: style="font-size: 28px;" -->

![](./assets/rust-attr-macro-vs-python-decorator.png) <!-- .element: height="300px" -->

![](./assets/rust-attr-macro.png) <!-- .element: height="200px" -->

---

<!-- .slide: data-background-color="#2d1414" -->

### Wails

![](./assets/wales.png) <!-- .element width="600px" -->

----

<!-- .slide: data-background-color="#2d1414" -->

In a nutshell... <!-- .element: style="font-size: 28px;" -->

![](./assets/webkit.png) <!-- .element height="250px" -->
![](./assets/go.png) <!-- .element height="270px" -->

Webkit (webview) + Go (runtime) <!-- .element: style="font-size: 32px;" -->

----

<!-- .slide: data-background-color="#2d1414" -->

#### Architecture

![](./assets/wails-architecture.png) <!-- .element width="650px" -->

JS Bindings...? <!-- .element: style="font-size: 26px;" -->

----

<!-- .slide: data-background-color="#2d1414" -->

This is actually similar to *Electron*<!-- .element: style="color: #2be0d7;" -->... But we just need *JS*<!-- .element: style="color: #f0b913;" --> and *Go*<!-- .element: style="color: #13b1f0;" --> to interop...
<!-- .element: style="font-size: 28px;" -->

Let's look at an example... <!-- .element: class="fragment" style="font-size: 28px;" -->

----

<!-- .slide: data-background-color="#2d1414" -->

Inside the main application...
<!-- .element: style="font-size: 30px;" -->

```go[4|8-10]
package main

func main() {
    wails.Run(&options.App{
        Title:  "Demo Desktop App",
        Width:  1024,
        Height: 768,
        Bind: []interface{}{
            &App{},
        },
    })
}

type App struct {
    // Some properties
}

```
<!-- .element: style="font-size: 18px; line-height: 1.3;" -->

Bind?... <!-- .element: class="fragment" style="font-size: 28px;" -->

----

<!-- .slide: data-background-color="#2d1414" -->

#### Method Binding

![](./assets/wails-method-binding.png)

```go
Bind: []interface{}{
  &App{},  // Instantiating an App struct
}
```
<!-- .element: style="font-size: 18px; line-height: 1.3;" -->

```go
// Inside App struct definition...

func (b *App) PowerfulFunction() {
    // Some complex logic
}
```
<!-- .element: style="font-size: 18px; line-height: 1.3;" -->

----

<!-- .slide: data-background-color="#2d1414" -->

And now we can directly use our `PowerfulFunction`<!-- .element: style="font-size: 28px; word-spacing: 3px; color: #dfdd53;" --> in frontend...
<!-- .element: style="font-size: 28px; word-spacing: 3px;" -->

```ts
import { PowerfulFunction } from "../wailsjs/go/main/App";

await PowerfulFunction();
```
<!-- .element: style="font-size: 18px; line-height: 1.3;" -->

What about events? Do they have events...? <!-- .element: class="fragment" style="font-size: 28px; word-spacing: 3px;" -->

----

<!-- .slide: data-background-color="#2d1414" -->

Of course they do... <!-- .element: style="font-size: 28px; word-spacing: 3px;" -->

![](./assets/wails-event.png) <!-- .element class="fragment" width="800px" -->

![](./assets/wails-event-emit.png) <!-- .element class="fragment" width="650px" -->

![](./assets/wails-event-on.png) <!-- .element class="fragment" width="650px" -->

---

### Developer Experience

![](./assets/dog-typing.gif) <!-- .element height="280px" -->
![](./assets/cat-typing.gif) <!-- .element height="280px" -->

----

I actually don't have slides for this section... <!-- .element: style="font-size: 28px; word-spacing: 3px;" -->

---

![](./assets/thank-you.gif) <!-- .element height="500px" -->