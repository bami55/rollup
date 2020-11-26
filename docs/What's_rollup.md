# Rollup

## Introduction

RollupはJavaScriptのモジュールバンドラー

CommonJSやAMDの代わりに、ES6で新しく標準化されたJavaScriptのESモジュールを使用する

ESモジュールを使用するとライブラリの個々の機能を自由かつシームレスに組み合わせることができる



## インストール

#### global

```sh
npm install --global rollup
```

#### local

npm or yarnでインストール

```sh
# npm
npm install rollup --save-dev
npx rollup --config

# yarn
yarn add -D rollup
yarn rollup --config
```

#### package.json

package.jsonにビルドコマンドのスクリプトを設定

```json
{
  "scripts": {
    "build": "rollup --config"
  }
}
```



## チュートリアル

### ①バンドルの作成

```javascript
// src/main.js
import foo from './foo.js';
export default function () {
  console.log(foo);
}
```

```javascript
// src/foo.js
export default 'hello world!';
```

#### バンドル作成

```sh
# コンソール出力（出力ファイルを指定していないため）
rollup src/main.js -f cjs

# ファイル出力
rollup src/main.js -o bundle.js -f cjs
```

#### 出力結果

```javascript
// bundle.js
'use strict';

const foo = 'hello world!';

const main = function () {
  console.log(foo);
};

module.exports = main;
```

#### 出力結果のコードを実行

```javascript
node
> var myBundle = require('./bundle.js');
> myBundle();
'hello world!'
```

### ②Configファイルの使用

Configでオプションを指定することができる

rootディレクトリに`rollup.config.js`を作成

```javascript
// rollup.config.js
export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  }
};
```

Configファイルを使用するには `--config` or `-c` のオプションをつける

```sh
rollup -c
```

Configファイルの設定を上書きして実行することもできる

```sh
rollup -c -o bundle-2.js # 出力ファイル名を "bundle-2.js" に変更
```

Configファイル `rollup.config.js` 以外のファイル名でも使用できる
本番環境、開発環境で切り替えたいときに便利

```sh
rollup --config rollup.config.dev.js
rollup --config rollup.config.prod.js
```

### ③プラグインの使用

