---
title: "Parcel With Elm"
date: 2018-01-29T21:47:09+08:00
---

Elm 虽然自带了 `elm-reactor` 和 `elm-make` 来开发和编译 Elm 项目，但是 `elm-reactor` 却不能在 html 内引入外部 css， 如果要使用第三方 UI 框架是没办法的，因为样式都引入不了，而 Elm 写样式的方式也不是直接写 css （当然，如果要写，就要可以在 html 内引入样式），如果官方工具都不支持，只能在 JS 现有的生态内寻找了。

当然一开始会想到用 webpack，但是 webpack 那复杂的配置实在让人不想多配置一次（当然 webpack 4 要改善这个情况），现在零配置可用的方案就是用 [Parcel](https://github.com/parcel-bundler/parcel)，虽然是零配置，也是可以通过 Plugin 来扩展的。网上搜索 Parcel 和 Elm 一起用的示例并不多，最终还是有解决方案的，使用 [parcel-plugin-elm](https://github.com/ssuman/parcel-plugin-elm) 即可。

Parcel 使用插件的方式十分简单，只需要安装好，因为 Parcel 规定了插件的包名需要以 parcel-plugin- 开头。使用 `yarn` 或者 `npm` 安装 parcel-plugin-elm：

```
yarn add parcel-plugin-elm
```

```
npm install parcel-plugin-elm
```

然后我们只需要在项目文件夹内加3个文件即可。

##### index.html:
```
<!DOCTYPE html>
<html>
<body>
    <div id="main">
    </div>
  <script src="./index.js"></script>
</body>
</html>
```

##### index.js:
```
import('./main.elm').then(Elm => {
    var mountNode = document.getElementById('main');
    var app = Elm.Main.embed(mountNode);
});
```

##### main.elm:
```
import Html exposing (..)
import Html.Events exposing (onClick)

model = 0

view model =  div [] 
    [ button [onClick Decrement] [text "-"] 
    ,text (toString model)
    , button [onClick Increment] [text "+2"]
    ]

type Message = Increment | Decrement

update msg model = 
    case msg of
        Increment -> model + 2
        Decrement -> model - 1

main = beginnerProgram
        {
            model = model,
            view = view,
            update = update
        }
```

然后在项目根目录下运行：

```
parcel index.html
```

再访问 [http://localhost:1234](http://localhost:1234)，就能看到 Elm 写出来的一个计数器的功能了。

没有什么配置和繁杂的东西，只需要一个 js 载入 Elm 程序，就像当时没有这些工程化的工具的时候写代码一样，新建一个 html 和 js 文件，前端开发就开始了。只是现在同时具备了简单的流程和工程化的能力。
