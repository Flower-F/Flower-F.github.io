---
title: CSS画棵动态圣诞树
date: 2021-12-22 23:45:38
tags: [css, react]
copyright: true
---
偶然在 b 站看到的视频，然后用 react 还原了一下，并部署到了 vercel。

仓库地址：
https://github.com/Flower-F/merry-christmas
在线体验：
https://merry-christmas-taupe.vercel.app/

```js
// App.js
import './App.css';

function App() {
  return (
    <div className="App">
      <div className="text">
        Merry Christmas
      </div>
      <div className="tree">
        <div className="iconfont icon-iconcollect-copy"></div>

        <div className="top">
          <span style={{ transform: 'rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(90deg) rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(180deg) rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(270deg) rotateX(30deg) translateZ(86px)' }}></span>
        </div>

        <div className="top" style={{ transform: 'translateY(78px)' }}>
          <span style={{ transform: 'rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(90deg) rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(180deg) rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(270deg) rotateX(30deg) translateZ(86px)' }}></span>
        </div>

        <div className="top" style={{ transform: 'translateY(156px)' }}>
          <span style={{ transform: 'rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(90deg) rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(180deg) rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(270deg) rotateX(30deg) translateZ(86px)' }}></span>
        </div>

        <div className="top" style={{ transform: 'translateY(234px)' }}>
          <span style={{ transform: 'rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(90deg) rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(180deg) rotateX(30deg) translateZ(86px)' }}></span>
          <span style={{ transform: 'rotateY(270deg) rotateX(30deg) translateZ(86px)' }}></span>
        </div>

        <div className="bottom">
          <span style={{ transform: 'translateZ(30px)' }}></span>
          <span style={{ transform: 'rotateY(90deg) translateZ(30px)' }}></span>
          <span style={{ transform: 'rotateY(180deg) translateZ(30px)' }}></span>
          <span style={{ transform: 'rotateY(270deg) translateZ(30px)' }}></span>
        </div>

        <span className="shadow"></span>
      </div>
    </div>
  );
}

export default App;
```

```css
/* App.css */

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.App, body {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
  background-color: #e8ffe8;
}

.tree {
  position: relative;
  width: 150px;
  height: 150px;
  transform-style: preserve-3d;
  transform: rotateX(-20deg) rotateY(30deg);
  animation: movie 7s linear infinite;
}

@keyframes movie {
  0% {
    transform: rotateX(-20deg) rotateY(360deg);
  }

  100% {
    transform: rotateX(-20deg) rotateY(0deg);
  }
}

.text {
  position: absolute;
  top: 20px;
  font-size: 2rem;
  font-weight: bold;
  color: #222;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.tree .icon-iconcollect-copy {
  position: absolute;
  left: calc(50% - 1rem) !important;
  top: -10rem !important;
  color: yellow !important;
  font-size: 2rem !important;
}

.tree div {
  position: absolute;
  top: -100px;
  left: 0;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
}

.tree .top span {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #69c069, #77dd77);
  clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
  transform-origin: bottom;
  border-bottom: 10px solid #00000019;
}

.tree .bottom span {
  position: absolute;
  top: 350px;
  left: calc(50% - 30px);
  width: 60px;
  height: 40%;
  background: linear-gradient(90deg, #bb4622, #df7214);
  /* clip-path: polygon(50% 0%, 0% 100%, 100% 100%); */
  transform-origin: bottom;
  border-bottom: 10px solid #00000055;
}

.tree .shadow {
  position: absolute;
  top: 0;
  left: 0;
  width: 150px;
  height: 150px;
  background-color: #0002;
  transform-style: preserve-3d;
  transform: rotateX(90deg) translateZ(-280px);
  filter: blur(10px);
}
```

参考资料：
https://www.youtube.com/watch?v=hrv2XAY27gU