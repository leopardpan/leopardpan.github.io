---
layout: post
title: vue和threeJS项目实践
date: 2023-09-04
description: "vue和threeJS项目实践"
tag: 前端
---

# 创建项目

```bash
nvm use 12
npm init vite@latest
# 选择vue + javascript
cd ProjectName
code .
npm install
npm install three
npm run dev
```

# 创建最简单的三元素

三元素：

1. 场景（物体）
2. 摄像机（相当于眼镜）
3. 渲染器

```js
<script setup>
import * as THREE from "three"

// 1. scene
const scene = new THREE.Scene();

// 2. camera
// 45: field of view; aspect ratio: widow.innerWidth / window.innerHeight; 0.1: near clipping plane; 1000: far clipping plane
const camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 1000);

// 3. renderer
const renderer = new THREE.WebGLRenderer({
  // 抗锯齿
  antialias: true
});
// set size
renderer.setSize(window.innerWidth, window.innerHeight);
// append to document to app block (webgl canvas)
document.getElementById('app').appendChild(renderer.domElement);

// render scence and camera
renderer.render(scene, camera);

</script>
```

现在的页面是一个黑黑的页面，因为我们还没有添加任何物体。同时，修改`style.css`，（删除多余的样式，只保留body的margin为0）让canvas占满整个页面,而不是出现滚动条。

效果如下：

![20230906114859](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230906114859.png)

# 创建物体

```js
// set background color,ox79deec is a light blue,0x表示16进制
renderer.setClearColor(0X79deec);

// 4 创建一个立方体，添加到场景中
const mygeometry = new THREE.BoxGeometry();
// 创建材质
const material = new THREE.MeshBasicMaterial({
  color: 0xff0000,
  // 设置线条呈现
  wireframe: true
});
//把材质渲染到立方体上,
const cube = new THREE.Mesh(mygeometry, material);
// 把立方体添加到场景中
scene.add(cube);
// 一定要设置相机的位置，否则看不到物体，否则摄像机都在原点上(0,0,0)
// x轴左右；y轴上下，z轴指向我们，我们需要把摄像机的z轴移动到靠近我们位置
camera.position.z = 5;
```

这时候，可以看到我们刚刚加进去的cube:

![20230906135823](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230906135823.png)

可以先把cube旋转一下：

```js
cube.rotation.y = 1;
// 拉高摄像头位置
camera.position.y = 5;
```

现在画面如下：

![20230906141053](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230906141053.png)


# 添加轨道控制器

为乐使得摄像机位置改变不影响物体在页面上的位置，需要添加一个轨道控制器：

```vue
import { OrbitControls} from  "three/examples/jsm/controls/OrbitControls"

//5 创建轨道控制器

const controls = new OrbitControls(camera,render.domElement);
```

# 添加动画


```js
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);
// 辅助网格
const gridHelper = new THREE.GridHelper(10, 10); // size,divisions
scene.add(gridHelper);
```





