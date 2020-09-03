---
title: 'Webpack 기본 개념'
date: 2020-09-03 15:09:96
category: javascript
thumbnail: { thumbnailSrc }
draft: false
---

## Webpack Concept

- 웹팩은 모던 자바스크립트 어플리케이션을 위한 모듈 번들러
- 웹팩은 어플리케이션을 처리 할 때 프로젝트에 필요한 모든 모듈을 매핑
- 하나 이상의 번들을 생성하는 **종속성 그래프(dependency graph)**를 내부적으로 빌드함

## What is dependency graph

- 하나의 파일이 다른 파일에 의존 할 때마다 웹팩은 이를 종속성으로 취급
- 이를 통해 웹팩은 이미지, 웹 글꼴과 같은 코드가 아닌 assets들을 가져와 애플리케이션에 대한 종속성으로 제공 할 수 있음
- 웹팩은 어플리케이션을 처리 할 때 command line 또는 config 파일에 정의된 `entry point`에서 시작함
- 이러한 진입점에서 시작하여 웹팩은 어플리케이션에 필요한 모든 모듈을 포함하는 종속성 그래프를 재귀적으로 빌드
- 이러한 모든 모듈을 브라우저에서 로드할 적은 수의 파일들(보통 하나)로 번들링함

## 왜 사용할까?

- 자바스크립트 코드가 많아지면 한 파일에 관리하기 어려워짐
- 코드의 유지보수가 쉽도록 코드를 모듈 단위로 나누어 관리하는 모듈 시스템 필요해짐
- 여러 개의 파일을 브라우저에서 로딩하는 것은 네트워크 비용이 많이 듬
- 각 파일은 서로의 스코프를 침범하지 않아야 하는데 잘못 작성될 경우 변수 충돌이 날 수 있음

## Core Concepts of Webpack

### Entry

- 진입점은 웹팩이 내부 종속성 그래프 작성을 시작하는 데 사용해야 하는 모듈을 나타냄
- 웹팩은 진입점이 직,간접적으로 의존하는 다른 모듈과 라이브러리를 알아냄
- 기본값은 `src/index.js`
- config을 통해 다른 값을 지정할 수 있음
- 프로젝트 루트에 `webpack.config.js`을 생성하고 아래와 같이 진입점을 명시함

```javascript
module.exports = {
  entry: './path/to/my/entry/file.js',
}
```

### Output

- 웹팩이 생성한 번들을 내보낼 위치와 이러한 파일의 이름을 지정
- 메인 output 파일의 기본값은 `./dist/main.js`,
- 다른 생성 파일은 `./dist` 폴더에 추가

```javascript
const path = require('path')

module.exports = {
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js',
  },
}
```

- `output.filename` : 번들 이름
- `output.path` : 번들을 내보낼 위치

### Loaders

- 기본적으로 웹팩은 JavaScript 및 JSON 파일만 이해함
- loader를 사용하여 웹팩이 다른 유형의 파일을 처리하고 이를 어플리케이션에서 사용하고 종속성 그래프에 추가 할 수 있는 유효한 모듈로 변환할 수 있음

```javascript
module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
}
```

- `test` : 변환해야 하는 파일을 식별
- `use` : 변환을 수행하는 데 사용해야하는 loader를 나타냄

> "야 웹팩 컴파일러, `require()`/`import`문 내에서 `.txt`파일로 해석되는 경로를 발견하면 번들에 추가하기 전에 `raw-loader`를 사용하여 변환해라."

### Plugins

- 번들 최적화, assets 관리 및 환경 변수 삽입과 같은 광범위한 작업을 수행

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  plugins: [new HtmlWebpackPlugin({ template: 'public/index.html' })],
}
```

- `html-webpack-plugin`은 생성된 모든 번들을 자동으로 삽입하여 어플리케이션용 HTML 파일을 생성

### Mode

- 매개 변수를 `development`, `production` 또는 `none`으로 설정
- 각 환경에 해당하는 웹팩의 내장 최적화를 활성화 할 수 있음
- 기본값은 `production`

```javascript
module.exports = {
  mode: 'production',
}
```

### Browser Compatibility

- 웹팩은 ES5를 준수하는 모든 브라우저를 지원
- IE8 이하는 지원되지 않음
- 웹팩에는 `import()` 및 `require.ensure()`에 대한 `Promise`가 필요함
- 이전 브라우저를 지원하려면 이러한 표현식을 사용하기 전에 polyfill을 로드해야함
