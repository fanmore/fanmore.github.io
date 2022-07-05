---
title: Threejs基础
categories: Ⅱ前端
sort: 2
tags:
  - 前端
  - 3D
date: 2022-07-05 09:22:29
updated: 2022-07-05 09:22:32
---

# 学习地址

[官方网站](https://threejs.org/)，可切换中文查看

## 环境搭建

先对某文件夹进行`npm init`初始化，然后安装[parceljs打包工具](https://parceljs.org/)，命令为`npm i parcel`，然后创建html入口并指定为打包目录

```js
"scripts": {
  "dev": "parcel src/index.html",
  "build": "parcel build src/index.html"
},
```

指定样式文件和js入口文件

```html
<link rel="stylesheet" href="./assets/css/style.css">
<script src="./assets/js/main.js" type="module"></script>
```

<!-- more -->

环境就搭建好了

# 安装Three

```
npm i three

引入
import * as THREE from 'three'
```

# Three基础流程

```js
import * as THREE from 'three'
import {
  OrbitControls
} from 'three/examples/jsm/controls/OrbitControls'

// console.log(THREE);

// 创建场景
const scene = new THREE.Scene()
// 创建透视相机：视野角度、长宽比、近端面、远端面
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000)
// 调整相机位置
camera.position.set(0, 0, 10)
// 把相机加入场景
scene.add(camera)

// 添加几何体
const geometry = new THREE.BoxGeometry(1, 1, 1);
// 材质
const material = new THREE.MeshBasicMaterial({
  color: 0x4194cd
});
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(geometry, material);
// 加入场景
scene.add(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer()
// 渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight)
// 将内容添加到body
document.body.appendChild(renderer.domElement)
// 使用渲染器将相机、场景进行渲染
// renderer.render(scene, camera)

// 创建轨道控制器：指定相机和渲染器
const controls = new OrbitControls(camera, renderer.domElement)

// 每帧重渲染
function render() {
  renderer.render(scene, camera)
  requestAnimationFrame(render)
}

render()
```

<img style="width:400px" src="/images/1656818378138.png"/>

# 坐标辅助器

能更方便开发时进行查看

```js
// 添加坐标轴辅助器，指定长度
const axesHelper = new THREE.AxesHelper(5)
// 加入场景
scene.add(axesHelper)
```

# 物体的位置和移动

```js
// 添加几何体及其大小
const geometry = new THREE.BoxGeometry(1, 1, 1);
// 材质及颜色
const material = new THREE.MeshBasicMaterial({
  color: 0x4194cd
});
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(geometry, material);
// 设置物体配置
cube.position.set(0, 0, 0)
// 或者直接设置单一位置
// cube.position.x = 1
// 加入场景
scene.add(cube);


// 每帧重渲染
function render() {

  cube.position.x += 0.01
  if (cube.position.x > 5) {
    cube.position.x = 0
  }

  renderer.render(scene, camera)
  requestAnimationFrame(render)
}
```

# 物体缩放和旋转

```js
// 缩放
// cube.scale.set(2, 1, 1)
// 或者
cube.scale.x = 3

// 旋转
cube.rotation.set(Math.PI / 8, 0, 0)
// 一直旋转
cube.rotation.x += 0.01
```

# requestAnimationFrame卡顿问题

requestAnimationFrame是尽可能的接近1帧的时间，但是受到电脑性能的影响，肯能出现跳帧的情况，所以在计算的时候最好使用帧的时间计算动画

```js
// 时钟运行时长
let time = clock.getElapsedTime()
// 下一次获取间隔时间
let deltaTime = clock.getDelta()
console.log('运行时长' + time)
console.log('获取间隔' + deltaTime)
```

# 阻尼

解决控制器的晃动问题

```js
// 创建轨道控制器：指定相机和渲染器
const controls = new OrbitControls(camera, renderer.domElement)
// 开启阻尼
controls.enableDamping = true

// 每帧重渲染
function render() {

  controls.update()

  renderer.render(scene, camera)
  requestAnimationFrame(render)
}
```

# 画面自适应和全屏

解决当容器大小变化后无法自适应问题

```js
// 自适应
window.addEventListener('resize', () => {
  // 更新摄像头
  camera.aspect = window.innerWidth / window.innerHeight
  // 更新摄像头投影矩阵
  camera.updateProjectionMatrix()
  // 更新渲染器
  renderer.setSize(window.innerWidth, window.innerHeight)
  // 更新渲染器像素比
  renderer.setPixelRatio(window.devicePixelRatio)
})
```

全屏

```js
// 全屏（非网页，而是指定元素）
window.addEventListener('dblclick', () => {
  if (document.fullscreenElement) {
    document.exitFullscreen()
  } else {
    renderer.domElement.requestFullscreen()
  }
})
```

