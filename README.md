# generosity-web-lint-rules

TypeScriptの可読性・保守性を高めるためのESLint共有設定です。  
ルールは`config/index.js`にまとまっており、Guard Clause推奨や命名規約など、`.eslintrc.js`で使っていたものをそのままパッケージ化しています。

## 使い方

`eslint.config.js` (または `eslint.config.mjs/ts`) で共有設定をそのままエクスポートしてください。

```js
// eslint.config.js (CommonJS)
const myRules = require('generosity-web-lint-rules');

module.exports = myRules;
```

```js
// eslint.config.js (ESM)
import myRules from 'generosity-web-lint-rules';

export default myRules;
```

もしくは、index.jsをそのままコピペして持ってくることもできます
```eslint.config.js
{
    plugins: {
      import: importPlugin,
    },
    rules: {
      /* -----------------------------------------------------------
       * 第II部：ループとロジックの単純化
       * 原則：制御フローを読みやすくする / ネストを浅くする / 早期リターン
       * ----------------------------------------------------------- */

      // elseブロックを使わず、早期リターン（Guard Clause）を推奨する
      // 「ネストを浅くする」ための強力なルールです。
      'no-else-return': ['error', { allowElseIf: false }],
```

TypeScriptを使わないプロジェクトでも動きますが、パーサとプラグインは peerDependencies として必須なので、インストールは忘れずに。`@typescript-eslint/*@8.47.0` は `eslint@^9.0.0` とセットで動くことを前提にしています。

## 動作確認フロー

1. `npm run lint:test`  
   `eslint --config config/index.js --print-config config/index.js` で共有設定が読み込めるか確認できます
2. `npm pack` → 別プロジェクトで `npm install ../generosity-web-lint-rules-*.tgz`  
   実際に `extends: ['generosity-web-lint-rules']` を書いて読み込めるか検証してください

## 公開前チェックリスト

- `package.json` の `version` を更新したか
- `npm publish --access public` で公開できることを `npm whoami` 等で確認したか
- 追加で必要なルールがあれば `config/index.js` に追記し、READMEも更新する

