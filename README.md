# 这个是干什么的

这个项目是一个 HTML + TailwindCSS 骨架结构和环境，用来快速的编写基于 TailwindCSS 的 HTML 页面。可以把 HTML 中的相关 Tailwind CSS Class 全部抽取并且实时编译成可以用的原生 CSS 代码。 支持页面监控和即时预览，每次修改 HTML 文件都会重新编译。配合 VSCode 的 Preview 功能可以即时查看页面变动。

# 依赖

- TailwindCSS V3
- NodeJS

TailwindCSS V 4.0.0 对于 CLI 做出更改，会自动检测项目内需要编译的源文件，不再使用 config 指定。实际测试这个功能存在 BUG。

# 使用

1. 切换到根目录，执行 `npm install`
2. 执行 `npm start dev`
3. 修改 public/index.html 同时开启 VSCode 的 Preview 功能即可实时查看更改效果。

# 各个文件的作用

## tailwind.config.js

1. 用来配置需要抽取 class 的 HTML 文件路径。默认只有一个首页，如果存在多个 HTML 或者其他 JS 文件，可以编辑 content 项添加。
2. 项目根目录存在此文件可以让 VSCode 识别到项目并且支持 class name 自动完成。

## package.json

用来定义 Node 项目，主要用途是用来运行自动监控、自动编译的 Script。本项目中会自动监控首页 HTML 文件，把其中的 class 抽取到 style.css 中。这里 all.css 是有些困惑的，其用途就是包含了 TailwindCSS 库中所有的 class，具体请看下节。

```Javascript
"scripts": {
    "dev": "tailwindcss -i ./all.css -o ./public/style.css --watch --poll --minify"
  },
```

注意：在 WSL 上运行，如果不使用 --poll 参数，watch 功能会失效。

## all.css

这个文件主要是使用了变量的方式，动态引入了 TailwindCSS 所有的 class。你可以认为这个文件就是一个包含 TailwindCSS 所有 class 的 css 文件。如果需要包含自定义的 css 则也是写入这个文件中。

## public

这个文件夹包含所有的可以上传到服务器的静态文件。编辑 HTML 结束后，仅仅把这个目录上传到 CDN 即可。

## index.html

这个是主页文件，大部分代码编辑工作将在这里完成。注意每次修改后，node 会自动更新 style.css 文件，但是 VSCode 的 Preview 功能可能会缓存 css 文件，所以有时候需要刷新才会看到最新的改变。

# 为什么不用 Play CDN

`<script src="https://cdn.tailwindcss.com"></script>`

Play CDN 在开发时候挺好用，但部署上线时候会有些麻烦。你不能把 Play CDN 用于生产环境，因为这个文件巨大加载很慢。
