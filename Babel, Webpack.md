ES6+ 또는 ES NEXT의 ES 최신 사양을 사용하여 프로젝트를 진행하려면 최신 사양으로 작성된 코드를 경우에 따라 IE를 포함한 구형 브라우저에서 문제 없이 동작시키기 위한 개발 환경을 구축하는 것이 필요하다. 특히 모듈의 경우, 모듈 로더가 필요하다.
<br/>

모던 브라우저에서는 ES6 모듈을 사용할 수 있다. 단, 아래와 같은 이유로 아직까지는 브라우저가 지원하는 ES6 모듈 기능보다는 Webpack 등의 모듈 번들러를 사용하는 것이 일반적이다.   

* IE를 포함한 구형 브라우저는 ES6 모듈을 지원하지 않는다.
* 브라우저의 ES6 모듈 기능을 사용하더라도 트랜스파일링이나 번들링이 필요하다.
* 아직 지원하지 않는 기능(Bare import 등)이 있다.
* 점차 해결되고는 있지만 아직 몇가지 이슈가 있다.

# Babel
```javascript
// ES6 화살표 함수와 ES7 지수 연산자
[1, 2, 3].map(n => n ** n);
```
<br/>

IE와 다른 구형 브라우저에서는 이 두가지 기능을 지원하지 않을 수 있다. Babel을 사용하면 위 코드를 아래와 같이 ES5 이하의 버전으로 변환할 수 있다.
<br/>

```javascript
// ES5
"use strict";

[1, 2, 3].map(function (n) {
  return Math.pow(n, n);
});
```
<br/>
<br/>

## Babel 설치
프로젝트에 따라 설정이 다를 수 있으므로 로컬로 설치한다.
<br/>

```bash
# package.json 생성
npm init -y
# babel-core, babel-cli 설치
npm install --save-dev @babel/core @babel/cli
```
<br/>
<br/>

## babelrc 설정 파일 작성
Babel을 사용하려면 @babel/preset-env을 설치해야 한다. @babel/preset-env은 함께 사용되어야 하는 Babel 플러그인을 모아 둔 것으로 Babel 프리셋이라고 부른다. Babel이 제공하는 공식 Babel 프리셋은 아래와 같다.

* @babel/preset-env
* @babel/preset-flow
* @babel/preset-react
* @babel/preset-typescript
@babel/preset-env도 공식 프리셋의 하나이며 필요한 플러그인 들을 프로젝트 지원 환경에 맞춰서 동적으로 결정해 준다. 프로젝트 지원 환경은 Browserslist 형식으로 .browserslistrc 파일에 상세히 설정할 수 있다. 프로젝트 지원 환경 설정 작업을 생략하면 기본값으로 설정된다. 기본 설정은 모든 ES6+ 코드를 변환한다.
<br/>

```bash
npm install --save-dev @babel/preset-env
```
<br/>
<br/>

프로젝트 루트에 babel.config.json 파일을 생성후 아래와 같이 작성한다.
```json
{
  "presets": ["@babel/preset-env"]
}
```
<br/>
<br/>

## 트랜스파일링
Babel을 사용하여 ES6+ 코드를 ES5 이하의 코드로 트랜스파일링하기 위해 Babel CLI 명령어를 사용할 수도 있으나 npm script를 사용할 수도 있다.   
package.json 파일에 scripts를 추가한다.
<br/>

script는 src/js 폴더(타깃 폴더)에 있는 모든 ES6+ 파일들을 트랜스파일링한 후, 그 결과물을 dist/js 폴더에 저장한다. 사용한 옵션의 의미는 아래와 같다.
* **-w**: 타깃 폴더에 있는 모든 파일들의 변경을 감지하여 자동으로 트랜스파일한다. (--watch 옵션의 축약형)
* **-d**: 트랜스파일링된 결과물이 저장될 폴더를 지정한다. (--out-dir 옵션의 축약형)
<br/>

```json
"scripts": {
    "build": "babel src/js -w -d dist/js"
  }
```
<br/>
<br/>

## Babel 플러그인
설치가 필요한 플러그인은 Babel 홈페이지에서 검색할 수 있다.   
검색한 플러그인은 아래와 같이 설치한다.
<br/>

```bash
npm install --save-dev @babel/plugin-proposal-class-properties
```
<br/>
<br/>

설치한 플러그인은 babel.config.json에 추가해야 한다.
<br/>

```json
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```
<br/>
<br/>

# Webpack
Webpack은 의존 관계에 있는 모듈들을 하나의 자바스크립트 파일로 번들링하는 모듈 번들러이다. Webpack을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요없다. 그리고 다수의 자바스크립트 파일을 하나의 파일로 번들링하므로 html 파일에서 script 태그로 다수의 자바스크립트 파일을 로드해야 하는 번거로움도 사라진다.
<br/>

Webpack이 자바스크립트 파일을 번들링하기 전에 Babel을 로드하여 ES6+ 코드를 ES5 코드로 트랜스파일링하는 작업을 실행하도록 설정한다. 그리고 Sass를 사용하는 경우, Sass 트랜스파일링도 Webpack에서 관리하도록 한다.
<br/>

## Webpack 설치

```bash
npm install --save-dev webpack webpack-cli
```
<br/>
<br/>

## babel-loader
Webpack이 모듈을 번들링할 때 Babel을 사용하여 ES6+ 코드를 ES5 코드로 트랜스파일링하도록 babel-loader를 설치한다.
<br/>

```bash
npm install --save-dev webpack webpack-cli
```
<br/>
<br/>

npm script를 아래와 같이 변경하여 Babel 대신 Webpack을 실행하도록 한다.
<br/>

```json
"scripts": {
    "build": "webpack -w"
}
```

## webpack.config.js
webpack.config.js은 Webpack이 실행될 때 참조하는 설정 파일이다. 프로젝트 루트에 webpack.config.js 파일을 생성하고 아래와 같이 작성한다.
<br/>

```javascript
const path = require('path');

module.exports = {
  mode: 'development',

  // enntry file
  entry: './src/app.js',

  // 컴파일 + 번들링된 js 파일이 저장될 경로와 이름 지정
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },

  //사용할 포트
  devServer: {
    compress: true,
    port: 9999
  },

  module: {
    rules: [
      {
        test: /\.js$/, //js파일만 포함하는 정규표현식
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            //사용할 preset과 plugin들을 설정한다.
            presets: ['@babel/preset-env', , '@babel/preset-react'],
            plugins: ['@babel/plugin-proposal-class-properties']
          }
        }
      }
    ]
  },

  devtool: 'source-map',
  // https://webpack.js.org/concepts/mode/#mode-development
  mode: 'development'
};
```
<br/>
<br/>
설정후 npm run build 커맨드로 실행하면 dist폴더에 bundle.js파일이 생성된다.
