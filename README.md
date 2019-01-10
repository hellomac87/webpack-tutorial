# Step by Step Webpack Configuration

## Intro
요즘 Frontend에서 Webpack은 번들링을 하는데 거의 필수적인 툴로 자리잡고 있다. 몇몇 프레임워크에서는 built-in으로 제공하여 `CRA(Create React App)`이나 `Vue-cli`등을 이용하면
별다른 설정없이 Webpack을 이용한 번들링 또한 가능하다. 때문에 Webpack을 이용해서 설정파일(Configuration) 작성이나, Release condition에 따른 분기 또는
최적화에 대해서 잘 모르는 사람이 간혹 보인다. 물론 0(Zero) Configuration 추세가 되면서 설정에 대한 피로도 줄이고자 하는 노력이 커뮤니티에서 일어나고 있긴 하지만,
설정 파일에 대한 이해 없이는 사용하는데 한계가 있다. 이 문서에서는 실제 프로덕션에서도 유연하게 사용할 수 있는 설정파일을 한땀 한땀 만들어 가면서, 필요한 부분에 대해 설명하고자 한다. 

## Step 1
> 사전 준비

설정 파일을 만들기 전에, `package.json` 부터 작성해 보도록 하자. 아래 명령어를 실행해서 `package.json` 파일을 만들자.
```bash
$ npm init
```
기본적인 `package.json` 파일이 만들어지는데, 여기선 대부분 중요하지 않으므로 `script` 부분을 제외하고 삭제하도록 하겠다.
```json
{
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```
이제 `webpack`을 설치해 보도록 하자. webpack 4.0부터는 `webpack-cli`가 필요하므로 같이 설치해 주도록 하자.
```bash
$ npm install webpack webpack-cli --save-dev
```
이제 폴더 구조를 간단하게 잡아 보도록 하겠다.
```
 |- configs      // webpack.config는 여기에 정의
 |- node_modules 
 |- src          // 개발 resources는 여기에 정의
    |- css
    |- img
    |- js
    |- index.html
 |- package.json   
```
간략하게 configs에는 빌드와 관련된 설정파일을 모아두고, src에는 번들될 리소스들을 모아두도록 하자. 이제 간단하게 빌드에 필요한 `js/css/image/html` 파일들을 만들어 두도록하자. 
#### src/js/index.js
```js
import '../css/index.css';
const title = 'Webpack Configuration';
const $el = document.getElementById('root');

$el.innerHTML = `<h1>Hello World! ${title}</h1>`;
```
#### src/css/index.css
```css
h1 {
    font-size: 50px;
    color: #6495ed;
}
```
#### src/img/image.png
이미지는 아래 샘플을 사용하도록 하자.
<img src= "https://user-images.githubusercontent.com/10627668/50545189-bb3f9700-0c4e-11e9-9099-e24ba11256bc.png">

#### src/index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=Edge">
    <title>Step by Step Webpack Configuration</title>
</head>
<body>
    <div id="root"></div>
    <script src="/js/bundle.js"></script>
</body>
</html>
```
따라서 다음과 같은 구조가 되어야 한다.
```
 |- configs      // webpack.config는 여기에 정의
 |- node_modules 
 |- src          // 개발 resources는 여기에 정의
    |- css
        |- index.jss
    |- img
        |- image.png
    |- js
        |- index.js
    |- index.html
 |- package.json   
```
