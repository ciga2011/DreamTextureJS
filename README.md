# DreamTexture.js - 3D模型自动纹理化开发包
[DreamTexture.js](https://tools.nsdt.cloud/DreamTexture) 是面向 three.js 开发者的 3D 模型纹理自动生成与设置开发包，可以为 webGL 应用增加 3D 模型的快速自动纹理化能力。

![image](https://github.com/ciga2011/DreamTextureJS/assets/3120837/7ce1fec1-ac28-48b5-8b56-8c0c8b5d44f3)


图一为原始模型, 图二图三为贴图后的模型。提示词：

> city, Realistic , cinematic , Front view ,Game scene graph

## 1、DreamTexture.js 开发包内容
DreamTexture.js 基于 Three.js 和稳定扩散(stable diffusion) AI 模型开发，用于实现 3D 模型的自动纹理化,当然版本 V1.0,主要文件及目录组织结构如下：

|开发包文件|	说明|
|-|-|
|dream-texture.cjs|	cjs 格式库文件|
|dream-texture.esm|	esm 格式库文件|
|dream-texture.umd|	umd 格式库文件|
|stable-diffusion-guide.md|	用于 DreamTexture.js 的稳定扩散服务安装指南|
|LICENSE.md|	开发包许可协议文件|
|example/|	DreamTexture.js 使用示例目录|

## 2、DreamTexture.js 开发包快速上手
以 ESM 库为例介绍如何使用 DreamTexture.js 开发包为 Three.js 应用增加 3D 模型的自动化纹理能力。

首先参考开发包中的稳定扩散服务安装指南部署自己的 stable diffusion api 服务，支持 windows 和 Linux。

接下来安装 three.js 开发环境，安装完成后需要引入 DreamTexture.js 库文件，以 ESM 库为例，引入代码如下：

```
import * as THREE from 'three';
import DreamTexture from './dream-texture.esm.min';
```

现在创建一个场景，在场景中导入 GLTF 模型 ，并可以适当的旋转或移动模型：
```
//将模型导入到场景
const gltfLoader = new THREE.GLTFLoader();
gltfLoader.load('monkey.glb', async (e) => {
  scene.add(e.scene);
});

// 将模型旋转到任何你想要的角度!
box.rotation.y = -Math.PI / 4;
```

然后实例化一个 DreamTexture 对象，注意要在参数中指定你的稳定扩散 API 服务的 URL：
```
//初始化DreamTexture对象，传入您的stable diffusion api 地址
const dt = new DreamTexture({
  baseUrl: 'http://127.0.0.1:7860', //stable diffusion url
});
```

现在就可以调用 DreamTexture 对象的 setTexture 方法传入提示词等参数, 让 AI 模型自动生成生成一张纹理图片，并投射到模型上，代码如下：
```
//编写提示词和其他参数
// 成功启动stable diffusion api后，可在 http://127.0.0.1:7860/docs 查看文档
const params = {
    prompt: 'monkey head, Brown hair, cartoon',//描述所需图像的细节越详细，Stable Diffusion生成效果越接近描述，较少描述则更具创意性。
    negative_prompt: 'blurry',//不希望Stable Diffusion生成的内容，用于排除不需要的元素。
    denoising_strength: 0.85,// 去噪强度
    cfg_scale: 15,//文字CFG比例
    image_cfg_scale: 7,//图片CFG比例
    steps: 10,//采样步数
    sampler_index: 'DPM++ SDE Karras',
    sampler_name: '',
};
dt.setTexture(scene, params).then((res) => {
  console.log('纹理添加成功！');
});
```

3D 模型的自动纹理化效果如下：

案例 1：

![image](https://github.com/ciga2011/DreamTextureJS/assets/3120837/582d93c6-eee5-4f63-ac6f-8e285855ea9e)


图一为原始模型, 图二图三为贴图后的模型。提示词：

> car, Realistic , photography , hyper quality , high detail , high resolution , Unreal Engine , Side view

案例 2：

![image](https://github.com/ciga2011/DreamTextureJS/assets/3120837/e5d0e615-e982-4982-bf2d-9294d0dd9b29)


图一为原始模型, 图二图三为贴图后的模型。图二提示词：

> Realistic , photography, bottle, porcelain

图三：将'porcelain'换为'glass'
> Realistic , photography, bottle, glass

## 3、DreamTexture.js 开发包 cjs/umd 库文件的使用
DreamTexture 支持三种常用的 js 库格式，除了前面介绍的 esm 格式，还支持 cjs、umd 格式：

cjs 库的引入代码如下：
```
const ProjectedMaterial = require('./dream-texture.cjs.js');
```
umd 库的引入代码如下：
```
<script src="./three.js"></script>
<script src="./dream-texture.umd.js"></script>
```

## 4、DreamTexture.js 开发包 API 接口说明
DreamTexture.js 的 API 接口非常简单，说明如下：

- new DreamTexture({ baseUrl })
  初始化 DreamTexture 对象,稍后用于 3D 模型的自动纹理化。

|参数|	描述|
|-|-|
|baseUrl|	stable diffusion api 地址|

- dreamTexture.setTexture(object3d:THREE.Object3D, params)
DreamTexture 会将传入的 object3d 的正视图作为依据来完成 3D 场景的自动纹理化，包括纹理的生成和自动投射。

|参数|	描述|
|-|-|
|object3d|	THREE.Object3D。支持 Group 和 Mesh。|
|params|	stable diffusion img2img api 的参数|
