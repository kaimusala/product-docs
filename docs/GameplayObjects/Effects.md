# 特效

**阅读本文大概需要 15 分钟。**

本文概述了特效的工作机制，展示在编辑器创建并使用特效的过程和特效在游戏中的应用。教程内容包含特效功能对象的属性面板和接口。

## 1. 什么是特效？

- 特效是指游戏中的特殊效果。比如说人物技能特效、场景瀑布、落叶、UI特效等等。
- 在游戏中，最为常见的为【粒子特效】，【粒子特效】是一种过程模型，将造型与动画结合为一个整体，用单个随时间变化的粒子作为场景的基本元素。本质上粒子特效是由一个或多个粒子发射器组成，并且每个粒子都有自己的生命周期和各种属性效果。

<video controls src="https://cdn.233xyx.com/1683524431908_929.mp4"></video>

## 2. 如何使用特效？

### 2.1 如何找到特效资源？

- 在【本地资源库】-【粒子特效】列表中找到大量的特效对象。

![](https://cdn.233xyx.com/athena/online/77dbc35b30f84191b4fa94bb46d46376_12166452.webp)

### 2.2 如何找到特效资源？

- 我们在找到特效资源后，可以根据需求找到自己想要的特效，并拖入场景中，修改其特效属性。

![](https://cdn.233xyx.com/athena/online/b73e9b1d30a842a0886d89066dfc9e35_12166458.webp)

- 基础属性中，存在播放/停止按钮，可以在编辑状态下，调整特效的播放状态。
- 在运行过程中，如果想要看到特效，需要开启【自动启用】功能。这样特效能够在运行时，自动播放特效。否则特效在运行状态下可能会看不见。

### 2.3 动态生成特效资源

![](https://cdn.233xyx.com/athena/online/b1b3e9b5c37443d0b47df6b64264afec_12166468.webp)

- 也可以将鼠标悬停在要的的资源上方，查看资源的GUID；也可以通过右键，复制对象的GUID。然后通过GUID动态创建相应的资源。
- 动态生成特效的示例脚本：

```ts
@Core.Class
export default class NewScript extends Core.Script {

    /** 当脚本被实例后，会在第一帧更新前调用此函数 */
    protected async onStart(): Promise<void> {
        //延迟3秒
        await TimeUtil.delaySecond(3)
        //生成特效
        let eff = await Gameplay.Particle.asyncSpawn({
            //特效的GUID
            guid: "4330",
            //特效同步设置为false
            replicates: false,
            //特效的生成位置
            transform: new Transform(new Vector(100, 0, 100), new Rotation(0, 0, 0),
                new Vector(2, 2, 1))
        }) as Gameplay.Particle
        //播放特效
        eff.play();
        //停止特效
        //eff.stop();
    }

}
```

- 效果图：

<video controls src="https://cdn.233xyx.com/1683524819944_206.mp4"></video>

## 3. 特效属性介绍

- 我们点击特效对象，就可以在属性面板(默认右下角)中编辑特效的属性面板。勾选自动启用，特效就会显示出来。以下展开介绍具体得特效属性：

![](https://cdn.233xyx.com/athena/online/1d40164a4c704dc68f478da556e18755_12166896.webp)

### 3.1. 特效资源

- 属性说明：特效资源指的是特效的资源ID，我们可以通过将【本地资源库】中的【特效资源】直接拖入到属性面板中实现更换资源，也可以通过更换资源ID更换相应的特效资源。
- 演示效果：

<video controls src="https://cdn.233xyx.com/1683527981425_522.mp4"></video>

### 3.2 自动启用

- 属性说明：设置特效是否在游戏开始时自动激活，是则会自动播放，否则需要脚本触发播放。
- 演示效果：

<video controls src="https://cdn.233xyx.com/1683528058177_392.mp4"></video>

### 3.3 生成粒子个数（Rate）

- 属性说明：特效粒子的生成数量
- 演示效果：

![](https://cdn.233xyx.com/1683528193261_248.gif)

- 当然我们也提供了【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。显示更加多变的效果。

![](https://cdn.233xyx.com/1683528241843_767.gif)

- 相关接口：

```ts
//设置特效生成粒子个数为3个
eff.setFloat("Rate",3)
//设置特效生成粒子个数在3到10之间随机取值
eff.setFloatRandom("Rate",10,3)
```

### 3.4 生命周期（LifeTime）

- 属性说明：特效粒子的生存时间
- 演示效果：

![](https://cdn.233xyx.com/1683528307644_518.gif)

- 当然我们也提供了【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。显示更加多变的效果。

![](https://cdn.233xyx.com/1683528632632_778.gif)

- 相关接口：

```ts
//设置特效生成周期为10
eff.setFloat("LifeTime",10)
//设置特效生成周期在10到100之间随机取值
eff.setFloatRandom("LifeTime",100,10)
```

### 3.5 大小（Size）

- 属性说明：特效粒子的大小
- 演示效果：

![](https://cdn.233xyx.com/1683528799147_205.gif)

- 当然我们也提供了【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。显示更加多变的效果。

![](https://cdn.233xyx.com/1683529283671_716.gif)

- 相关接口：

```ts
//设置特效大小为10
eff.setVector("Size",new Type.Vector(10,10,10))
//设置特效大小在10到20之间随机取值
eff.setVectorRandom("Size",new Type.Vector(20,20,20)，new Type.Vector(10,10,10))
```

### 3.6 速度（Speed）

- 属性说明：特效粒子的方向与速度
- 演示效果：

![](https://cdn.233xyx.com/1683530016922_082.gif)

- 当然我们也提供了【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。显示更加多变的效果。

![](https://cdn.233xyx.com/1683530132633_194.gif)

- 相关接口：

```ts
//设置特效在Z轴方向的速度为50
eff.setVector("Speed",new Type.Vector(0,0,50))
//设置特效在Z轴方向的速度为在0到50之间随机取值
eff.setVectorRandom("Speed",new Type.Vector(0,0,50)，new Type.Vector(0,0,0))
```

### 3.7 发射器位置（EmitterLocation）

- 属性说明：特效粒子的生成位置
- 演示效果：由于固定位置不好分辨，我们以随机位置做演示效果
- 随机效果依旧是以【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。

![](https://cdn.233xyx.com/1683530340730_107.gif)

- 相关接口：

```ts
//设置特效的播放位置为（0,0,0）
eff.setVector("EmitterLocation",new Type.Vector(0,0,0))
//设置特效的播放位置在（0,0,0）到（50,50,50）之间随机取值
eff.setVectorRandom("EmitterLocation",new Type.Vector(50,50,50)，new Type.Vector(0,0,0))
```

### 3.8 颜色（InitialColor）

- 属性说明：特效粒子的颜色
- 演示效果：颜色比较简单，我们将随机颜色与正常颜色放在一起展示。
- 随机颜色依旧是【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。

![](https://cdn.233xyx.com/1683530381157_532.gif)

- 相关接口：

```ts
//设置特效的颜色，其中Type.LinearColor各参数分别代表红、绿、蓝和透明度
eff.setColor("Color", new Type.LinearColor(1,0,0,1))
//设置特效的颜色，并在红色和绿色之间随机生成
eff.setColorRandom("Color",new Type.LinearColor(1,0,0,1)，new Type.LinearColor(0,1,0,1))
```

### 3.9 初始旋转角度（Rotion）

- 属性说明：特效粒子的初始旋转角度
- 演示效果：旋转角度比较简单，我们将随机角度与正常角度放在一起展示。
- 随机角度依旧是【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。

![](https://cdn.233xyx.com/1683535008248_004.gif)

- 相关接口：

```ts
//设置特效初始旋转角度为0.5
eff.setFloat("Rotion",0.5)
//设置特效初始旋转角度在0到1之间随机取值
eff.setFloatRandom("Rotion",1,0)
```

### 3.10 旋转速度（RotationRate）

- 属性说明：特效粒子的旋转速度
- 演示效果：旋转速度比较简单，我们将随机旋转速度与正常旋转放在一起展示。
- 随机旋转速度依旧是【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。

![](https://cdn.233xyx.com/1683535222123_545.gif)

- 相关接口：

```ts
//设置特效旋转速度为0.5
eff.setFloat("RotationRate",0.5)
//设置特效旋转速度在0到1之间随机取值
eff.setFloatRandom("RotationRate",1,0)
```

### 3.11 阻力（Drag）

- 属性说明：特效粒子的受到的阻力效果
- 演示效果：阻力比较简单，我们将随机阻力与正常阻力放在一起展示。
- 随机阻力依旧是【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。

![](https://cdn.233xyx.com/1683535246840_829.gif)

- 相关接口：

```ts
//设置特效阻力为1
eff.setFloat("Drag",10)
//设置特效阻力在0到1之间随机取值
eff.setFloatRandom("Drag",1,0)
```

### 3.12 重力（Acceleration）

- 属性说明：特效粒子的受到的重力效果
- 演示效果：

![](https://cdn.233xyx.com/1683535280025_608.gif)

- 当然我们也提供了【范围随机】的属性，即每个粒子特效都是在设定范围内，进行随机生成。

![](https://cdn.233xyx.com/1683535358013_913.gif)

- 相关接口：

```ts
//设置特效在Z轴方向受到的重力为-50
eff.setVector("Acceleration",new Type.Vector(0,0,-50))
//设置特效在Z轴方向受到的重力在0到-50之间随机取值
eff.setVectorRandom("Acceleration",new Type.Vector(0,0,-50)，new Type.Vector(0,0,0))
```

### 3.13 循环/循环次数（即将废弃）

- 【循环】属性说明：特效是否循环，开启后特效将不会停下来，进行循环播放。
- 【循环次数】属性说明：当开启循环时，循环次数不生效。当取消循环时，特效会根据循环次数，进行播放，播放次数到达循环次数后，特效将会停止播放。
- 演示效果：

<video controls src="https://cdn.233xyx.com/1683535407198_168.mp4"></video>

- 备注：该属性接口即将废弃，特效循环由开发者，通过脚本调用play函数实现。

## 4. 属性显示问题

- 为了让用户体验到更加自由的特效功能，暴露了以上的特效属性，特效属性会根据特效的特点，暴露相关的属性，也就是说，以上特效属性在不同的特效，显示是不同的。有的会显示1个，有的会显示多个。并且我们不会翻新老特效的资源，只有新制作的特效才会显示这些属性。
