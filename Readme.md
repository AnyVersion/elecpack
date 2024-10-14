# elecpack

## Install

```bash
npm install @anyversion/elecpack
```

## Usage

[Demo](https://github.com/gityoog/elecpack-demo)

```typescript
import { ElecpackBuilder } from '@anyversion/elecpack'

const builder = new ElecpackBuilder({
  main: {
    context: string // main file context
    entry: string // main file entry
    electron: string? // electron path, default: require('electron')
    checker?: boolean // enable typescript checker, default: true
    assets?: string // assets folder, use ElecpackRuntime.getAsset(name) to get asset file
    define?: Record<string, any> // define for ElecpackRuntime.define
    bytecode?: boolean | 'all' | 'production' | 'development' // enable bytecode, default: false
    configFile?: string | string[] // extra webpack config files
    skipDefConfigFile?: boolean // skip default webpack config file, default: false
  }
  preload: {
    context: string // preload file context
    entry: Record<string, string> // preload file entry, use ElecpackRuntime.getPreload(name) to get preload file
    assets?: string | { from: string, to: string } // assets folder
    define?: Record<string, any> // define for Webpack define plugin
    configFile?: string | string[]  // extra webpack config files
    bytecode?: WebpackBuilder.Bytecode // enable bytecode, default: false
    skipDefConfigFile?: boolean // skip default webpack config file, default: false
  }
  renderer: {
    context: string // renderer file context
    entry: Record<string, {
      entry: string | string[] // entry file
      html: HtmlWebpackPlugin.Options // html-webpack-plugin options
    }>  // renderer file entry, use ElecpackRuntime.getRenderer(name) to get renderer file or ElecpackRuntime.load({ name: name, hash?: string }, bw)
    assets?: string | { from: string, to: string } // assets folder
    define?: Record<string, any> // define for Webpack define plugin
    configFile?: string | string[] // extra webpack config files
    bytecode?: WebpackBuilder.Bytecode // enable bytecode, default: false
    skipDefConfigFile?: boolean // skip default webpack config file, default: false
  }

  // for worker or other files
  files?: Record<string, {
    context: string
    entry: entry
    assets?: string | { from: string, to: string }
    define?: Record<string, any>
    configFile?: string | string[]
    bytecode?: WebpackBuilder.Bytecode
    skipDefConfigFile?: boolean
  }> // use ElecpackRuntime.getFiles(name) to get file

  // config for electron-builder
  electronBuilder?: {
    name: 'elecpack-example', // output name
    version: '1.0.0', // output version
    configuration?: {
      // electron-builder configuration [docs](https://www.electron.build/configuration)
    }
  }
})

// for development
builder.startDev()

// for production
builder.startBuild()
```

```typescript
import ElecpackRuntime from "@anyversion/elecpack/runtime";

const bw = new BrowserWindow({
  webPreferences: {
    preload: ElecpackRuntime.getPreload("preload"),
  },
});
ElecpackRuntime.load({ name: "demo", hash: "hash" }, bw);

console.log(ElecpackRuntime.define["key"]);

new Worker(ElecpackRuntime.getFiles("test-worker"));
```
