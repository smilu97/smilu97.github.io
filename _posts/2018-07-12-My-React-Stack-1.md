---
layout: post
title: "My custom React stack-1"
date: 2018-06-17 23:09:20 +0900
description: React를 이용한 SPA개발에 내가 주로 사용하는 오픈소스 스택에 대한 정리
---
# React

리액트는 기본적으로는 HTML DOM에 원하는 DOMNode(?)를 동적으로 그려주는 프레임워크이다.

여기서 동적이란, state라고 부르게 될 특정 오브젝트가 변할 때 마다 DOMNode도 새로 그려지는 것을 말한다.

그리고 이 새로 그리는 작업은 때때로 아주 많은 개수의 Node를 re-render해야 하는 상황일 수 있는데, 이 때 re-render를 효율적으로 하기 위해서 React는 내부적으로 Virtual DOM이라는 것을 관리하고, Virtual DOM에 새로운 DOMNode들을 모두 그린 후에 한꺼번에 실제 DOM에 적용한다고 한다.

이런 방식은 Double buffering과 굉장히 유사한데, 변경된 Node를 하나씩 적용하는 것보다 연산이 줄어든다고 한다.

그리고 state가 변경되는 것에 대해서 어떻게 추적할지에 대한 해답을 간단하다, state를 변경할 때는 꼭 React에서 마련해준 인터페이스 (setState 함수)를 통해야 한다고 약속하는 것이다.

정리하자면, React는 setState함수가 불릴 때 마다 (물론 맨 처음에도 한번) state를 업데이트 한 후에 새로운 state를 DOMNode들로 매핑하고 모두 Virtual DOM에 적용한 후에, 완전히 그려진 Virtual DOM을 실제 DOM에 적용해준다. 이것이 React가 제공해주는 가장 중요한 기능일 것이다.

## Project setting

리액트의 프로젝트 세팅에는 여러가지가 들어가서 어렵기로 유명하다.

우선 거의 필수적으로, Babel을 이용해서 JSX 문법을 사용가능하도록 만들 필요가 있다. 또, Webpack을 이용해서 작동하는 JS파일들이나 기타 이미지, 영상 파일등을 하나의 파일로 번들링할 필요가 있다.

위의 작업들을 위해 npm에서 설치해야 하는 패키지들은 대략 이렇다

* react
* react-dom
* webpack-cli
* babel-cli
* babel-preset-react
* babel-preset-es2015
* babel-preset-stage-0
* html-loader

위에 나열해놓은 것들 외에도 리액트 프로젝트를 구축하는 데 있어서 굉장히 많은 모듈들이 포함될 수 있는데, 각각의 역할과 사용법을 잘 알고 있다면 프로젝트 구축이 어렵지 않겠지만 나는 모두 잘 아는 편은 아니기 떄문에 삽질의 경험과 얕은 공부로 해결했다.

### 1. npm init

(이 단계는 사실 너무나 기본적인 것이라 많은 사람들한테 너무나 당연하게 보이겠지만 그래도 일단 적어놓도록 하겠음.)

[Node](https://nodejs.org/)에서 우선 Node를 받아 설치해야 한다.

Node가 설치되고 나면 OSX, Linux등을 사용하는 사람들의 경우 Terminal, 윈도우 사용자들의 경우 Command창에서 `node`, `npm`, `npx` 이 세 가지 명령어들이 사용가능하도록 되었을 것이다. [기본 Terminal 사용법](https://medium.com/@psychet_learn/cli-cli-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-%EB%B0%8F-%EC%82%AC%EC%9A%A9%EB%B2%95-c8d000ebc162)

`node -v`를 터미널에 타이핑했을 때 뭔가 버전같은 텍스트가 보인다면 정상적으로 node가 깔린 것이다. 만약 그렇지 않다면 이 뒤의 것들이 모두 불가능 할 것이다.

원하는 곳에 적당한 이름의 폴더를 만들고 나서 그 폴더에서 터미널을 켜자.

`npm init` 명령어를 사용하면 이 폴더를 npm이 관리하는 프로젝트 폴더로 만들 수 있다. 뭔가 이것저것 물어볼 건데 모르겠으면 대충 엔터로 넘겨도 된다. 필자의 경우 전부 엔터로 넘긴다.

### 2. 필요한 패키지들 설치

어떤 패키지들이 필요할지는 때에 따라 다양하게 변화할 수 있는데 여기서는 일단 기본적인 것들만 설치하도록 한다.

```sh
npm install react # React
npm install react-dom # HTML에 React를 사용할 수 있게 해줌
npm install --dev webpack-cli # 개발한 소스코드나 이미지, 영상등을 모두 한 파일로 묶어줌
npm install --dev babel-loader # webpack이 babel을 사용할 수 있도록 해줌
npm install --dev babel-cli # 개발할 때 최신버전 혹은 특수한 문법을 사용할 수 있도록 해주는 데 필요한 기본
npm install --dev babel-preset-react # 리액트를 위한 특수한 문법을 사용할 수 있도록 해줌
npm install --dev babel-preset-es2015 # 2015년 최신 자바스크립트 문법을 사용할 수 있도록 해줌
npm install --dev babel-preset-stage-0 # stage-0 까지의 안정적인 문법을 사용할 수 있도록 해줌
```

위의 내용들을 그냥 복사해서 터미널에 붙여놓으면 된다. # 이후의 내용들은 주석이니 터미널에서 무시해준다. 그냥 읽어보고 신경쓰지 않아도 된다.

### 3. 기본 HTML 생성

```html
<!DOCTYPE html>
<!--[if lt IE 7]>      <html class="no-js lt-ie9 lt-ie8 lt-ie7"> <![endif]-->
<!--[if IE 7]>         <html class="no-js lt-ie9 lt-ie8"> <![endif]-->
<!--[if IE 8]>         <html class="no-js lt-ie9"> <![endif]-->
<!--[if gt IE 8]><!--> <html class="no-js"> <!--<![endif]-->
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title></title>
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
        <!--[if lt IE 7]>
            <p class="browsehappy">You are using an <strong>outdated</strong> browser. Please <a href="#">upgrade your browser</a> to improve your experience.</p>
        <![endif]-->
        <div id="root"></div>
        <script src="bundle.js"></script>
    </body>
</html>
```

대충 위와 같이 필수적인 것들만 담긴 빈 HTML을 만들어준다. 필자는 프로젝트 폴더에 dist폴더를 만들고 그 안에 `index.html`로 만들었다.

나중에 이 HTML위에 우리가 만든 React앱이 올라갈 것이다.

bundle.js를 로딩하려 하는 것을 마지막에 볼 수 있는데, 저 파일은 webpack이 나중에 자동으로 만들어줄 것이다.

### 4. 첫 소스코드 파일 생성

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

/*
 * id가 root인 Element를 찾는다.
 * 이 Element는 아까 만든 HTML을 보면 body에 들어있다.
 */
const NODE = document.getElementById('root');

/*
 * HTML에 그려넣을 Component
 */
function RootComponent() {
    return (
        <div>
            Hello React!
        </div>
    );
}

/*
 * RootComponent를 그려서 NODE안에 그린다.
 */
ReactDOM.render(
    <RootComponent />,
    NODE,
);
```

앱이 실행할 가장 처음이 될 소스코드이다. 필자는 프로젝트 폴더 바로 위에 `index.js`로 만들었다.

### 5. webpack 설정

```javascript
var path = require('path');

exports.default = {
    entry: './index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js',
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /(node_modules|bower_components)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['react', 'es2015', 'stage-0']
                    }
                }
            },
        ],
    },
}
```

이번에도 파일을 하나 새로 만드는데, 꼭 이름을 `webpack.config.js`로 하고 프로젝트 폴더 바로 위에 놓아야 한다.

entry는 번들링할 프로젝트의 가장 첫 번째 실행될 소스코드를 말한다.

output은 번들링한 파일이 어디에 저장될지 알려준다.

module은 발견한 import들에 대해서 어떤 처리를 더할 것인지 알려준다.

### End. 잘 보이는지 확인하기

이제 프로젝트 폴더에서 `npx webpack`을 실행하면 dist폴더에 bundle.js가 자동생성 되어있을 것이다. 이 상태에서 index.html을 한번 열어서 잘 나오는지 확인해보자.

이 정도가 React 프로젝트 구축의 가장 기본적인 부분들이다. 가장 기본이 이렇게 귀찮고 배워야 할 게 많다는 점에 대해서 많은 개발자들이 공감했다.

하지만 걱정할 필요가 없다. 사실 create-react-app을 이용하면 여태까지 해왔던 것들 + Live hot-reloading local Server, more Webpack modules 까지 많은 설정들을 자동으로 해준다.

### [create-react-app](https://github.com/facebook/create-react-app)

```sh
npm install -g create-react-app
npx create-react-app react-project-name
```

저 위의 명령어 2개를 react-project-name대신 원하는 이름을 집어넣어서 실행시키기만 하면 해당 이름으로 리액트 프로젝트를 구축해준다.

