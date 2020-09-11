---
title: '기존 프로젝트에 Typescript 도입하기 - Use separate parsers for js and ts'
date: 2020-09-11 15:09:77
category: react-native
thumbnail: { thumbnailSrc }
draft: false
---

## Typescript 점진적 도입

- 기존 Javascript로 작성된 React Native 프로젝트
- 코드량이 엄청나게 많고 거쳐 간 손도 많아 유지보수가 어려워지고 있음
- 코드의 가독성을 높이고 디버깅이 좀더 쉬워지도록 하기 위해 Typescript 도입
- But, 워낙 코드량이 많기 때문에 한꺼번에 변경하기 어려움
- js, ts가 공존할 수 있도록 프로젝트를 세팅하고 조금씩 옮겨가기로 함

## Eslint

- 기존의 eslint는 parser로 `babel-eslint` 사용했음
- Typescript를 같이 사용하기 위해서는 `.eslintrc.js` 파일을 수정해야 함
- 기존 js 파일들은 `babel-eslint`, ts는 `@typescript-eslint/parser`로 사용
- `overrides`에 분리해서 작성

```javascript
module.exports = {
  env: {
    browser: true,
    es6: true,
    jest: true,
    'react-native/react-native': true,
  },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:prettier/recommended',
  ],
  globals: {
    Atomics: 'readonly',
    SharedArrayBuffer: 'readonly',
  },
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
      legacyDecorators: true,
    },
    ecmaVersion: 2018,
    sourceType: 'module',
  },
  plugins: ['react', 'react-hooks', 'react-native', 'prettier'],
  rules: {
    // 원하는 rule 적용
  },
  parser: 'babel-eslint',
  settings: {
    react: {
      version: 'detect',
    },
  },
  // ts 파일들을 위한 parser 분리
  overrides: [
    {
      files: ['.ts', 'app/**/*.ts', 'app/**/*.tsx'],
      env: {
        browser: true,
        es6: true,
        jest: true,
        'react-native/react-native': true,
      },
      extends: [
        'eslint:recommended',
        'plugin:react/recommended',
        'plugin:@typescript-eslint/eslint-recommended',
        'plugin:@typescript-eslint/recommended',
        'plugin:@typescript-eslint/recommended-requiring-type-checking',
        'plugin:prettier/recommended',
      ],
      globals: {
        Atomics: 'readonly',
        SharedArrayBuffer: 'readonly',
      },
      parserOptions: {
        project: './tsconfig.json',
        ecmaFeatures: {
          jsx: true,
        },
        ecmaVersion: 2018,
        sourceType: 'module',
      },
      plugins: [
        'react',
        'react-hooks',
        'react-native',
        '@typescript-eslint',
        'prettier',
      ],
      rules: {
        // 원하는 rule 적용
      },
      parser: '@typescript-eslint/parser',
    },
  ],
}
```

## 참고

- [How to run typescript-eslint on .ts files and eslint on .js files in the same project in VSCode](https://stackoverflow.com/questions/57597142/how-to-run-typescript-eslint-on-ts-files-and-eslint-on-js-files-in-the-same-pr)
