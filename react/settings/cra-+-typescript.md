# CRA + Typescript

## CRA Typescript Template

`npx create-react-app my-app --template typescript`

## TSConfig

```javascript

```

## ESLint

* **eslint**
* **@typescript-eslint/parser**
* **@typescript-eslint/eslint-plugin**
* **eslint-plugin-react**

```text
npm i -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
npm i -D eslint-plugin-react
```

### .eslintrc.js

```javascript
module.exports =  {
    parser:  '@typescript-eslint/parser', 
    extends:  [
        'plugin:@typescript-eslint/recommended', 
        'plugin:prettier/recommended',
        'prettier'  
    ],
    parserOptions:  {
    ecmaVersion:  2018, 
    sourceType:  'module', 
    ecmaFeatures:  {
      jsx:  true,  
    },
    },
    rules:  {
    // 추후 .prettierrc.js 파일에서 설정해줄 예정
    },
    settings:  {
      react:  {
        version:  'detect', 
      },
    },
  };
```

## Prettier

* **prettier**
* **eslint-config-prettier:** Disables ESLint rules that might conflict with prettier
* **eslint-plugin-prettier:** Runs prettier as an ESLint rule

```text
npm i -D prettier eslint-config-prettier eslint-plugin-prettier
```

{% hint style="warning" %}
**`prettier/@typescript-eslint`** has been _****_[_**removed**_](https://github.com/prettier/eslint-config-prettier/blob/main/CHANGELOG.md#version-800-2021-02-21)  _****_in **`eslint-config-prettier`** v8.0.0

. Just remove it from your ESLint config file.

[https://stackoverflow.com/questions/65675771/eslint-couldnt-find-the-config-prettier-typescript-eslint-after-relocating](https://stackoverflow.com/questions/65675771/eslint-couldnt-find-the-config-prettier-typescript-eslint-after-relocating)
{% endhint %}

### .prettierrc.js

```javascript
module.exports =  {
    "singleQuote": true, 
    "semi": true,
    "useTabs": false,
    "tabWidth": 2,
    "trailingComma": "all",
    "printWidth": 80,
    "arrowParens": "always",
    "orderedImports": true,
    "bracketSpacing": true,
    "jsxBracketSameLine": false,
    "arrowParens": "avoid",
  };
```

## VSCode Setting

```javascript
{
  "eslint.autoFixOnSave": true, // has been deprecated, use editor.codeActionsOnSave 								    // instead 라고 써있었지만 주석 처리를 하면 저장 시 										// formatting이 안되어서 주석을 풀었다.
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    { "language": "typescript", "autoFix": true },
    { "language": "typescriptreact", "autoFix": true }
  ],
  {
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.formatOnSave": false
  },
  "[javascriptreact]": {
    "editor.formatOnSave": false
  },
  "[typescript]": {
    "editor.formatOnSave": false
  },
  "[typescriptreact]": {
    "editor.formatOnSave": false
  }
}
```

