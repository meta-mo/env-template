## インストール

- `git clone git@github.com:meta-mo/sample-env.git`
- `cd sample-env && yarn`
- `yarn dev`

## 技術スタック

- Vue 3
- TypeScript
- Vite
  - なぜ Vite?
    - Vue CLI はファイルの更新毎にバンドルするので遅い
- Tailwind
- ESLint
- Prettier
- StyleLint
- husky
- lint-staged

## VSCode の設定

VSCode での開発体験を良くする ESLint,Prettier の為の設定
保存、もしくは CLI などでコマンドを打つことで整形される

### 拡張機能の追加

便利

- [Prettier - Code formatter - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
  - Prettier が VSCode のコードを整形してくれる
- [ESLint - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
  - ESLint のエラー等々を VSCode に表示してくれる
- [Formatting Toggle - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=tombonnike.vscode-status-bar-format-toggle)
  - 整形したくないファイルを開いたときにトグルで無効化できる

### settings.json の編集

- `ctrl` + `shift` + `p` > `Preferences: Open Settings(JSON) | 基本設定: 設定(JSON)を開く` を選択
- 以下の設定を追加

```json
  /* eslintを動かすファイルを設定 */
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact",
    "vue"
  ],
  /* VSCodeの組み込みフォーマッターを使わずにPrettierを使う */
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  /* 貼り付け、保存、入力した時にフォーマットする */
  "editor.formatOnPaste": true,
  "editor.formatOnSave": true,
  "editor.formatOnType": true,
  /* 保存した時に整形する */
  "editor.codeActionsOnSave": [
    "source.organizeImports",
    "source.fixAll.eslint",
    "source.fixAll.stylelint"
  ],
  /* stylelintを動かすための設定 */
  "css.validate": false,
  "scss.validate": false,
  "stylelint.enable": true,
  "stylelint.packageManager": "yarn"
```

### 導入しているルールやパッケージ

#### ESLint

```js
module.exports = {
  env: {
    node: true, // node環境で利用する
    browser: true // browser環境で利用する
  },
  extends: [
    'plugin:vue/vue3-recommended', // vue3で使える基本ルール
    'eslint:recommended', // JavaScriptで使える基本ルール
    'plugin:eslint-comments/recommended', // ESLintを除外する際のコメントのルール
    '@vue/typescript/recommended', // TypeScriptで使える基本ルール
    'prettier' // PrettierとESLint整合性ををいい感じに保ってくれる
  ],
  plugins: [
    'node', // nodeで使えるルールなどを追加してくれる
    'unicorn' // いろいろなルールなどを追加してくれる
  ],
  rules: {
    /* ここから下は@vue/typescript の恐らくバグで存在しない基本ルールを追加している */
    'constructor-super': 'off',
    'getter-return': 'off',
    'no-const-assign': 'off',
    'no-dupe-args': 'off',
    'no-dupe-class-members': 'off',
    'no-dupe-keys': 'off',
    'no-func-assign': 'off',
    'no-import-assign': 'off',
    'no-new-symbol': 'off',
    'no-obj-calls': 'off',
    'no-redeclare': 'off',
    'no-setter-return': 'off',
    'no-this-before-super': 'off',
    'no-undef': 'off',
    'no-unreachable': 'off',
    'no-unsafe-negation': 'off',
    'no-var': 'error',
    'prefer-const': 'error',
    'prefer-rest-params': 'error',
    'prefer-spread': 'error',
    'valid-typeof': 'off'
  }
}
```

### Prettier

ここは好み

```json
{
  "useTabs": false, // インデントにタブを使用しない
  "tabWidth": 2, // 1インデントあたりの空白数
  "semi": false, // 関数の最後にセミコロンはいらない
  "singleQuote": true, // 文字列はシングルクォートにする
  "trailingComma": "none", // 配列などの最後にカンマをいれない、入れたほうが良い気もする
  "printWidth": 100 // 折り返す行の長さ、100文字
}
```

### StyleLint

SCSS で使う場合を想定

```js
module.exports = {
  extends: [
    'stylelint-config-standard', // いくつかの基本ルール
    'stylelint-prettier/recommended', // PrettierとStyleLint整合性ををいい感じに保ってくれる
    'stylelint-config-recess-order' // CSSのプロパティをソートしてくれる
  ],
  syntax: 'scss', // scssで使う
  plugins: [
    'stylelint-scss', // scssで使えるように
    'stylelint-order', // order順を変更できるように
    'stylelint-prettier' // prettierの為の設定
  ],
  rules: {
    'at-rule-no-unknown': null, // Tailwindなどと干渉するので消す
    'no-descending-specificity': null, // 「詳細度が高いセレクタを後に書かなければならない」が苦労するので消す
    'prettier/prettier': true, // prettierを使う
    'order/order': [['dollar-variables', 'declarations', 'rules']], // CSSの並び順のルール、
    'color-hex-length': 'short' // カラーコードを短縮できるときは短縮
  }
}
```

### huskey & lint-staged

Git のステージに上がっているファイルを対象に eslint を実行する
