# 027版本API改动

#### [优化]统一使用命名空间`mw`并兼容省略

027版本中对所有原先的范围较大的命名空间例如【Gameplay】、【UI】、【Type】等进行**命名空间隐藏处理**。部分范围较小的命名空间例如【Events】、【SystemUtil】、【StringUtil】**新增同名类对象，静态封装之前的方法**。所有API统一使用`mw`命名空间且支持省略命名空间的调用方式以缩短调用链。

```TypeScript
// 027修改前
InputUtil.onKeyDown(Type.Keys.Enter, async () => {
    let particle = await Core.GameObject.asyncSpawn({guid: "1001"}) as Gameplay.Particle;
    particle.worldLocation = Type.Vector.zero;
    particle.worldRotation = Type.Rotation.zero;
    particle.worldScale = Type.Vector.zero;
    particle.play();
});

// 027修改后
InputUtil.onKeyDown(Keys.Enter, async () => {
    let particle = await GameObject.asyncSpawn("1001") as Effect;
    particle.worldTransform.position = Vector.zero;
    particle.worldTransform.rotation = Rotation.zero;
    particle.worldTransform.scale = Vector.zero;
    particle.play();
});
```

#### [修改]隐藏内部接口Mobile Editor

#### [修改]删除资源预加载字段`preloadAsset`

由于资源相关改动，027版本不再支持代码使用`preloadAssets`属性去预加载资源（临时方案设计不合理）。脚本中若有使用需要删除否则会检测出编译报错。提前下载资源的方式只保留优先加载栏。027针对该改动做了兼容方案，所有老项目使用`preloadAsset`去预加载的资源，当在027版本编辑器打开项目时会自动进行读取导入至优先加载栏中，保证与之前效果相同。下图显示027对比026优先加载栏内多出的资源。

![img](https://arkimg.ark.online/1695721497041-1.webp)

#### [修改] 枚举类型

| 枚举名称（老）    | 枚举名称（新） | 修改方案 |
| ----------------- | -------------- | -------- |
| AssetType         | -              | 删除     |
| HideInEditorState | -              | 删除     |
| RepType           | -              | 删除     |

#### [删除]`<类>`AbilityObject 能力对象

- 删除低频使用对象【AbilityObject】

#### [修改]`<类>`AbilityState 能力状态

- 删除低频使用对象【AbilityState】

#### [删除]`<类>`AccountService 账号服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。
- 删除`getOpenId()`方法，去除OpenId概念，后续凡是与账号相关服务统一使用【Player】下`userId`属性。

| 接口名称（老） | 属类（老）     | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | -------------- | ----------------- | -------------- | ---------- | -------- |
| getInstance    | AccountService | MethodDeclaration | -              |            |          |
| getOpenId      | AccountService | MethodDeclaration | -              |            |          |

#### [修改]`<类>`AdsService 广告服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。
- 删除`show()`方法，替换为`showAd()`。

| 接口名称（老） | 属类（老） | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ---------- | ----------------- | -------------- | ---------- | -------- |
| getInstance    | AdsService | MethodDeclaration | -              |            |          |
| show           | AdsService | MethodDeclaration | showAd         | AdsService |          |

#### [修改]`<类>`AdvancedVehicle 高级轮式载具

- 删除废弃方法`getGearRatio`，替换为`getGearData`。
- 删除设置方法`setDriver`、`setSimulatePhysics`、`setSteeringInput`、`setThrottleInput`，替换为标准属性setter实现：`owner`、`simulatePhysics`、`steeringInput`、`throttleInput`。

| 接口名称（老）     | 属类（老）      | 接口类型（老）    | 接口名称（新）  | 属类（新）      | 修改方案 |
| ------------------ | --------------- | ----------------- | --------------- | --------------- | -------- |
| getGearRatio       | AdvancedVehicle | MethodDeclaration | getGearData     | AdvancedVehicle |          |
| setDriver          | AdvancedVehicle | MethodDeclaration | owner           | AdvancedVehicle |          |
| setSimulatePhysics | AdvancedVehicle | MethodDeclaration | simulatePhysics | AdvancedVehicle |          |
| setSteeringInput   | AdvancedVehicle | MethodDeclaration | steeringInput   | AdvancedVehicle |          |
| setThrottleInput   | AdvancedVehicle | MethodDeclaration | throttleInput   | AdvancedVehicle |          |

#### [删除]`<类>`Anchor 空锚点

- 因【Anchor】 空锚点运行态暂不具备任何特殊功能，且内部没有独立接口，删除对象。

#### [修改]`<类>`AnalyticsService 分析服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。

| 接口名称（老） | 属类（老）       | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ---------------- | ----------------- | -------------- | ---------- | -------- |
| getInstance    | AnalyticsService | MethodDeclaration | -              |            |          |

#### [删除]`<类>`AICharacter AI角色

- 删除【AICharacter】类，统一使用【Character】类代表角色对象，消除角色对象之间的区别。
- 删除内部私有`defaultV1EditorDataJson`属性。
- 删除`setServerMovementEnable`方法和`serverCalculateEnable`属性，替换为【Character】类中新增属性`complexMovementEnabled`。`complexMovementEnabled`默认为true，开启移动组件计算。`complexMovementEnabled`为false，关闭移动组件计算。

| 接口名称（老）          | 属类（老）  | 接口类型（老）      | 接口名称（新）         | 属类（新） | 修改方案 |
| ----------------------- | ----------- | ------------------- | ---------------------- | ---------- | -------- |
| setServerMovementEnable | AICharacter | MethodDeclaration   | complexMovementEnabled | Character  |          |
| defaultV1EditorDataJson | AICharacter | PropertyDeclaration | -                      |            |          |
| serverCalculateEnable   | AICharacter | SetAccessor         | complexMovementEnabled | Character  |          |

#### [修改]`<类>`Animation 动画

- 027版本由【Animation】对象承担角色播放动画的功能
- `onAnimFinished`事件委托替换为`onFinish`，去除时态。
- `rate`属性替换为`speed`

| 接口名称（老） | 属类（老） | 接口类型（老） | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ---------- | -------------- | -------------- | ---------- | -------- |
| onAnimFinished | Animation  | GetAccessor    | onFinish       | Animation  |          |
| rate           | Animation  | GetAccessor    | speed          | Animation  |          |
| rate           | Animation  | SetAccessor    | speed          | Animation  |          |

#### [修改]`<namespace>`AssetUtil 函数

- 命名空间【AssetUtil】改为类【AssetUtil】。
- 删除冗余接口`isAssetExist()`和`isAssetLoaded()`，使用`assetLoaded()`代替。
- 删除冗余接口`loadAsset()`，`asyncDownloadAsset()`会自动加载已下载资源。

| 接口名称（老）     | 属类（老） | 接口类型（老）      | 接口名称（新）     | 属类（新） | 修改方案 |
| ------------------ | ---------- | ------------------- | ------------------ | ---------- | -------- |
| assetLoaded        | -          | FunctionDeclaration | assetLoaded        | AssetUtil  | 替换     |
| asyncDownloadAsset | -          | FunctionDeclaration | asyncDownloadAsset | AssetUtil  | 替换     |
| isAssetExist       | -          | FunctionDeclaration | assetLoaded        | AssetUtil  | 替换     |
| isAssetLoaded      | -          | FunctionDeclaration | assetLoaded        | AssetUtil  | 替换     |
| loadAsset          | -          | FunctionDeclaration | asyncDownloadAsset | AssetUtil  | 替换     |

#### [修改]`<类>`AvatarSettings 人物设置

| 接口名称（老）        | 属类（老）     | 接口类型（老）    | 接口名称（新）      | 属类（新）     | 修改方案 |
| --------------------- | -------------- | ----------------- | ------------------- | -------------- | -------- |
| enableOptimization    | AvatarSettings | MethodDeclaration | optimizationEnabled | AvatarSettings |          |
| isOptimizationEnabled | AvatarSettings | MethodDeclaration | optimizationEnabled | AvatarSettings |          |

#### [修改]`<类>`Base 基类

- `guid`属性修改为`gameObjectId`
- 删除对象的无效属性`useUpdate`（仅保留脚本的属性）。
- 删除对象生命周期方法`onDestroy`、`onReplicated`、`onStart`和`onUpdate`（仅保留脚本的方法）。
- 删除单双端判定方法`isRunningClient`（仅保留脚本的方法）。
- 删除内部接口`arrayDefaultItem`。

| 接口名称（老）   | 属类（老） | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| ---------------- | ---------- | ----------------- | -------------- | ---------- | -------- |
| guid             | Base       | GetAccessor       | gameObjectId   | Base       | 替换     |
| useUpdate        | Base       | GetAccessor       | -              |            | 删除     |
| useUpdate        | Base       | SetAccessor       | -              |            | 删除     |
| arrayDefaultItem | Base       | MethodDeclaration | -              |            | 删除     |
| isRunningClient  | Base       | MethodDeclaration | -              |            | 删除     |
| onDestroy        | Base       | MethodDeclaration | -              |            | 删除     |
| onReplicated     | Base       | MethodDeclaration | -              |            | 删除     |
| onStart          | Base       | MethodDeclaration | -              |            | 删除     |
| onUpdate         | Base       | MethodDeclaration | -              |            | 删除     |

#### [删除]`<类>`BlockingArea 禁行区

- 原先的【BlockingArea】修改为【BlockingVolume】，空间区域统一使用Volume。
- 删除失效属性`playerStateResponse`。
- 删除针对角色设置通行权限的方法`setCurrentPlayerPassable`和`getCurrentPlayerPassable`。后续统一使用同一个接口控制【GameObject】的通行权限。

`setCurrentPlayerPassable：false`使用`removePassableTarget`进行替换。

`setCurrentPlayerPassable：true`使用`addPassableTarget`进行替换。

`getCurrentPlayerPassable`使用`getTargetPassable`进行替换。

| 接口名称（老）              | 属类（老）   | 接口类型（老）    | 接口名称（新）    | 属类（新）     | 修改方案 |
| --------------------------- | ------------ | ----------------- | ----------------- | -------------- | -------- |
| getCurrentPlayerPassable    | BlockingArea | MethodDeclaration | getTargetPassable | BlockingVolume |          |
| setBlockAllPlayer           | BlockingArea | MethodDeclaration | clear             | BlockingVolume |          |
| setCurrentPlayerPassable    | BlockingArea | MethodDeclaration | addPassableTarget | BlockingVolume |          |
| setNonCharacterActorCanPass | BlockingArea | MethodDeclaration | addPassableTarget | BlockingVolume |          |

#### [删除]`<类>`BlockingAreaManager 禁行区管理

- 删除【BlockingAreaManager】，不再提供全局禁行区对象的管理类。 

#### [删除]`<类>`CameraShake 摄像机抖动

- 【CameraShake】类修改为【CameraShakeInfo】接口，迁移其中关于抖动数据的属性。
- 【CameraShake】类中的`start`方法迁移至新增【Camera】对象中已静态方法的方式实现。

| 接口名称（老） | 属类（老）  | 接口类型（老）    | 接口名称（新）     | 属类（新）      | 修改方案 |
| -------------- | ----------- | ----------------- | ------------------ | --------------- | -------- |
| pitchAmplitude | CameraShake | MethodDeclaration | rotationYAmplitude | CameraShakeInfo |          |
| pitchFrequency | CameraShake | MethodDeclaration | rotationYFrequency | CameraShakeInfo |          |
| rollAmplitude  | CameraShake | MethodDeclaration | rotationXAmplitude | CameraShakeInfo |          |
| rollFrequency  | CameraShake | MethodDeclaration | rotationXFrequency | CameraShakeInfo |          |
| start          | CameraShake | MethodDeclaration | switch             | Camera          |          |
| xAmplitude     | CameraShake | MethodDeclaration | positionXAmplitude | CameraShakeInfo |          |
| xFrequency     | CameraShake | MethodDeclaration | positionXFrequency | CameraShakeInfo |          |
| yAmplitude     | CameraShake | MethodDeclaration | positionYAmplitude | CameraShakeInfo |          |
| yFrequency     | CameraShake | MethodDeclaration | positionYFrequency | CameraShakeInfo |          |
| yawAmplitude   | CameraShake | MethodDeclaration | rotationZAmplitude | CameraShakeInfo |          |
| yawFrequency   | CameraShake | MethodDeclaration | rotationZFrequency | CameraShakeInfo |          |
| zAmplitude     | CameraShake | MethodDeclaration | positionZAmplitude | CameraShakeInfo |          |
| zFrequency     | CameraShake | MethodDeclaration | positionZFrequency | CameraShakeInfo |          |

#### [删除]`<类>`CameraSystem 摄像机系统

- 原有【CameraSystem】替换为【Camera】，将摄像机与玩家角色解耦并继承【GameObject】成为游戏对象。支持场景摆放，挂载卸载和动态生成销毁切换等操作，实现逻辑上的多相机。
- 解构摄像机系统：弹簧臂+镜头，抽象`SpringArm`弹簧臂对象包裹原先实际操作弹簧臂的接口。每个`Camera`摄像机对象持有一个`SpringArm`弹簧臂对象。
- 新增`switch`静态方法在多个相机对象之间进行切换，切换过程可以通过参数自定义摄像机运动。同时新增`onSwitchComplete`事件委托，用户可以待摄像机切换完成时执行绑定函数。此外删除`moveByPath`方法，后续摄像机路径移动可以通过`switch`方法代替。
- 删除摄像机抖动对象`cameraShake`，通过Camera类调用新增`shake`/`stopShake`静态方法抖动/停止抖动摄像机，抖动数据【CameraShakeInfo】和抖动时间作为参数传入。
- 删除摄像机锁定方法：`cameraLockTarget`、`cancelCameraLockTarget`、`setCameraLockTarget`。【Camera】新增`lock`和`unlock`方法控制摄像机锁定对象。此外删除`lockTargetOffset`属性，该属性的功能需要使用锁定方法中的`lockOffset`参数代替。此外新增`lookAt`方法执行方便用户执行单次锁定。
- 删除摄像机聚焦方法：`cameraFocusing`。删除摄像机聚焦属性：`cameraFocusEnable`。聚焦动作可通过用户设置摄像机坐标或者使用`switch`静态方法实现。
- 删除原先透视开关`fadeEffectEnable`和透明度`fadeEffectValue`接口，使用`fadeObstructionEnabled`和`fadeObstructionOpacity`代替。此外删除`enableFadeEffect`废弃接口。
- 删除摄像机投影模式（后续只有透视模式）以及相关功能：`cameraProjectionMode`、`orthoFarClipPlane`、`orthoNearClipPlane`、`orthoWidth`。如果需要使用正交模式(例如制作固定视角休闲游戏),推荐使用视场=35度(或更小)，并同时调大摄像机距离等参数来模拟正交模式。
- 删除摄像机移动碰撞检测相关接口：`enableMovementCollisionDetection`、`movementCollisionDuration`、`movementCollisionMinLocationDelta`。
- 删除遮挡检测接口：`occlusionDetectionEnable`。
- 删除摄像机跟随接口：`followTargetEnable`、`followTargetInterpSpeed`、`cancelCameraFollowTarget`、`setCameraFollowTarget`，后续摄像机通过使用【Gaemobject】的`parent`属性去挂载/卸载其他对象来实现跟随。
- 删除摄像机抬高接口：`raiseCameraEnable`和`raiseCameraHeight`，后续摄像机开启弹簧臂碰撞时会伸缩，而关闭弹簧臂碰撞时可以选择透视。
- 删除摄像机模拟真实晃动开关属性`realEffectEnable`。
- 删除摄像机整体参数设置接口：`applySettings`和`getCurrentSettings`。
- 删除摄像机覆盖控制器旋转接口：`setOverrideCameraRotation`和`resetOverrideCameraRotation`。玩家控制器旋转由【Player】新增的`setControllerRotation`和`getControllerRotation`替换。

| 接口名称（老）                     | 属类（老）   | 接口类型（老）      | 接口名称（新）                    | 属类（新） | 修改方案 |
| ---------------------------------- | ------------ | ------------------- | --------------------------------- | ---------- | -------- |
| cameraCollisionEnable              | CameraSystem | GetAccessor         | springArm.collisionEnabled        | Camera     |          |
| cameraCollisionInterpSpeed         | CameraSystem | GetAccessor         | springArm.collisionInterpSpeed    | Camera     |          |
| cameraDownLimitAngle               | CameraSystem | GetAccessor         | downAngleLimit                    | Camera     |          |
| cameraFOV                          | CameraSystem | GetAccessor         | fov                               | Camera     |          |
| cameraFocusEnable                  | CameraSystem | GetAccessor         | -                                 |            |          |
| cameraLocationLagEnable            | CameraSystem | GetAccessor         | positionLagEnabled                | Camera     |          |
| cameraLocationLagSpeed             | CameraSystem | GetAccessor         | positionLagSpeed                  | Camera     |          |
| cameraLocationMode                 | CameraSystem | GetAccessor         | positionMode                      | Camera     |          |
| cameraProjectionMode               | CameraSystem | GetAccessor         | -                                 |            |          |
| cameraRelativeTransform            | CameraSystem | GetAccessor         | localTransform                    | Camera     |          |
| cameraRotationLagEnable            | CameraSystem | GetAccessor         | rotationLagEnabled                | Camera     |          |
| cameraRotationLagSpeed             | CameraSystem | GetAccessor         | rotationLagSpeed                  | Camera     |          |
| cameraRotationMode                 | CameraSystem | GetAccessor         | rotationMode                      | Camera     |          |
| cameraSystemRelativeTransform      | CameraSystem | GetAccessor         | springArm.localTransform          | Camera     |          |
| cameraSystemWorldTransform         | CameraSystem | GetAccessor         | springArm.worldTransform          | Camera     |          |
| cameraUpLimitAngle                 | CameraSystem | GetAccessor         | upAngleLimit                      | Camera     |          |
| cameraWorldTransform               | CameraSystem | GetAccessor         | worldTransform                    | Camera     |          |
| enableMovementCollisionDetection   | CameraSystem | GetAccessor         | -                                 |            |          |
| fadeEffectEnable                   | CameraSystem | GetAccessor         | fadeObstructionEnabled            | Camera     |          |
| fadeEffectValue                    | CameraSystem | GetAccessor         | fadeObstructionOpacity            | Camera     |          |
| fixedCameraZAxis                   | CameraSystem | GetAccessor         | fixedElevation                    | Camera     |          |
| followTargetEnable                 | CameraSystem | GetAccessor         | -                                 |            |          |
| followTargetInterpSpeed            | CameraSystem | GetAccessor         | -                                 |            |          |
| lockTargetOffset                   | CameraSystem | GetAccessor         | -                                 |            |          |
| movementCollisionDuration          | CameraSystem | GetAccessor         | -                                 |            |          |
| movementCollisionMinLocationDelta  | CameraSystem | GetAccessor         | -                                 |            |          |
| occlusionDetectionEnable           | CameraSystem | GetAccessor         | -                                 |            |          |
| orthoFarClipPlane                  | CameraSystem | GetAccessor         | -                                 |            |          |
| orthoNearClipPlane                 | CameraSystem | GetAccessor         | -                                 |            |          |
| orthoWidth                         | CameraSystem | GetAccessor         | -                                 |            |          |
| raiseCameraEnable                  | CameraSystem | GetAccessor         | -                                 |            |          |
| raiseCameraHeight                  | CameraSystem | GetAccessor         | -                                 |            |          |
| slotOffset                         | CameraSystem | GetAccessor         | localTransform.position           | Camera     |          |
| targetArmLength                    | CameraSystem | GetAccessor         | springArm.length                  | Camera     |          |
| targetOffset                       | CameraSystem | GetAccessor         | springArm.localTransform.position | Camera     |          |
| transform                          | CameraSystem | GetAccessor         | worldTransform                    | Camera     |          |
| usePawnControlRotation             | CameraSystem | GetAccessor         | springArm.useControllerRotation   | Camera     |          |
| cameraCollisionEnable              | CameraSystem | SetAccessor         | springArm.collisionEnabled        | Camera     |          |
| cameraCollisionInterpSpeed         | CameraSystem | SetAccessor         | springArm.collisionInterpSpeed    | Camera     |          |
| cameraDownLimitAngle               | CameraSystem | SetAccessor         | downAngleLimit                    | Camera     |          |
| cameraFOV                          | CameraSystem | SetAccessor         | fov                               | Camera     |          |
| cameraFocusEnable                  | CameraSystem | SetAccessor         | -                                 |            |          |
| cameraLocationLagEnable            | CameraSystem | SetAccessor         | positionLagEnabled                | Camera     |          |
| cameraLocationLagSpeed             | CameraSystem | SetAccessor         | positionLagSpeed                  | Camera     |          |
| cameraLocationMode                 | CameraSystem | SetAccessor         | positionMode                      | Camera     |          |
| cameraProjectionMode               | CameraSystem | SetAccessor         | -                                 |            |          |
| cameraRelativeTransform            | CameraSystem | SetAccessor         | localTransform                    | Camera     |          |
| cameraRotationLagEnable            | CameraSystem | SetAccessor         | rotationLagEnabled                | Camera     |          |
| cameraRotationLagSpeed             | CameraSystem | SetAccessor         | rotationLagSpeed                  | Camera     |          |
| cameraRotationMode                 | CameraSystem | SetAccessor         | rotationMode                      | Camera     |          |
| cameraSystemRelativeTransform      | CameraSystem | SetAccessor         | springArm.localTransform          | Camera     |          |
| cameraSystemWorldTransform         | CameraSystem | SetAccessor         | springArm.worldTransform          | Camera     |          |
| cameraUpLimitAngle                 | CameraSystem | SetAccessor         | upAngleLimit                      | Camera     |          |
| cameraWorldTransform               | CameraSystem | SetAccessor         | worldTransform                    | Camera     |          |
| enableMovementCollisionDetection   | CameraSystem | SetAccessor         | -                                 |            |          |
| fadeEffectEnable                   | CameraSystem | SetAccessor         | -                                 |            |          |
| fadeEffectValue                    | CameraSystem | SetAccessor         | -                                 |            |          |
| fixedCameraZAxis                   | CameraSystem | SetAccessor         | fixedElevation                    | Camera     |          |
| followTargetEnable                 | CameraSystem | SetAccessor         | -                                 |            |          |
| followTargetInterpSpeed            | CameraSystem | SetAccessor         | -                                 |            |          |
| lockTargetOffset                   | CameraSystem | SetAccessor         | -                                 |            |          |
| movementCollisionDuration          | CameraSystem | SetAccessor         | -                                 |            |          |
| movementCollisionMinLocationDelta  | CameraSystem | SetAccessor         | -                                 |            |          |
| occlusionDetectionEnable           | CameraSystem | SetAccessor         | -                                 |            |          |
| orthoFarClipPlane                  | CameraSystem | SetAccessor         | -                                 |            |          |
| orthoNearClipPlane                 | CameraSystem | SetAccessor         | -                                 |            |          |
| orthoWidth                         | CameraSystem | SetAccessor         | -                                 |            |          |
| raiseCameraEnable                  | CameraSystem | SetAccessor         | -                                 |            |          |
| raiseCameraHeight                  | CameraSystem | SetAccessor         | -                                 |            |          |
| realEffectEnable                   | CameraSystem | SetAccessor         | -                                 |            |          |
| slotOffset                         | CameraSystem | SetAccessor         | localTransform.position           | Camera     |          |
| targetArmLength                    | CameraSystem | SetAccessor         | springArm.length                  | Camera     |          |
| targetOffset                       | CameraSystem | SetAccessor         | springArm.localTransform.position | Camera     |          |
| usePawnControlRotation             | CameraSystem | SetAccessor         | springArm.useControllerRotation   | Camera     |          |
| cameraShake                        | CameraSystem | PropertyDeclaration | -                                 |            |          |
| currentCameraMode                  | CameraSystem | PropertyDeclaration | -                                 |            |          |
| enableFadeEffect                   | CameraSystem | PropertyDeclaration | -                                 |            |          |
| occludeCameraActor                 | CameraSystem | PropertyDeclaration | -                                 |            |          |
| applySettings                      | CameraSystem | MethodDeclaration   | -                                 |            |          |
| attachCameraToCharacterCapsuleSlot | CameraSystem | MethodDeclaration   | attachToSlot                      | Character  |          |
| attachCameraToCharacterMeshSlot    | CameraSystem | MethodDeclaration   | parent                            | Camera     |          |
| attachToGameObject                 | CameraSystem | MethodDeclaration   | -                                 |            |          |
| cameraFocusing                     | CameraSystem | MethodDeclaration   | -                                 |            |          |
| cameraLockTarget                   | CameraSystem | MethodDeclaration   | lock                              | Camera     |          |
| cancelCameraFollowTarget           | CameraSystem | MethodDeclaration   | parent                            | Camera     |          |
| cancelCameraLockTarget             | CameraSystem | MethodDeclaration   | unlock                            | Camera     |          |
| getCurrentSettings                 | CameraSystem | MethodDeclaration   | -                                 |            |          |
| getDefaultCameraShakeData          | CameraSystem | MethodDeclaration   | -                                 |            |          |
| moveByPath                         | CameraSystem | MethodDeclaration   | -                                 |            |          |
| resetOverrideCameraRotation        | CameraSystem | MethodDeclaration   | -                                 |            |          |
| screenShock                        | CameraSystem | MethodDeclaration   | shake                             | Camera     |          |
| setCameraFollowTarget              | CameraSystem | MethodDeclaration   | parent                            | Camera     |          |
| setCameraLockTarget                | CameraSystem | MethodDeclaration   | lock                              | Camera     |          |
| setOverrideCameraRotation          | CameraSystem | MethodDeclaration   | setControllerRotation             | Player     |          |
| startCameraShake                   | CameraSystem | MethodDeclaration   | shake                             | Camera     |          |
| stopCameraShake                    | CameraSystem | MethodDeclaration   | stopShake                         | Camera     |          |
| switchCameraMode                   | CameraSystem | MethodDeclaration   | preset                            | Camera     |          |

#### [修改]`<类>`Character 角色

- 为消除角色对象之间的区别，统一使用【Character】类代表场景中的角色对象，将原先【CharacterBase】的基础能力进行抽象并封装进【Character】。
- 删除原先【Character】中的技能接口：`onSkillTriggered`等。使用【InputUtil】代替。
- 删除属性`cameraSystem`，将摄像机与玩家角色解耦并继承【GameObject】成为游戏对象。

| 接口名称（老）    | 属类（老） | 接口类型（老） | 接口名称（新） | 属类（新） | 修改方案 |
| ----------------- | ---------- | -------------- | -------------- | ---------- | -------- |
| onSkill1Triggered | Character  | GetAccessor    | onKeyUp        | InputUtil  |          |
| onSkill2Triggered | Character  | GetAccessor    | onKeyUp        | InputUtil  |          |
| onSkill3Triggered | Character  | GetAccessor    | onKeyUp        | InputUtil  |          |
| onSkill4Triggered | Character  | GetAccessor    | onKeyUp        | InputUtil  |          |
| onSkill5Triggered | Character  | GetAccessor    | onKeyUp        | InputUtil  |          |

#### [删除]`<类>`CharacterBase 角色基类

- 为消除角色对象之间的区别，统一使用【Character】类代表场景中的角色对象，将原先【CharacterBase】代表角色基础能力的接口迁移进【Character】。
- 删除空中灵活度控制接口：`airControlBoostMultiplier`和`airControlBoostVelocityThreshold`，同时空中灵活度接口`airControl`替换为`driftControl`；空中减速接口`brakingDecelerationFalling`替换为`horizontalBrakingDecelerationFalling`。减低理解难度。
- 删除角色动画模式`animationMode`，用户不再需要手动切换。动画模式会根据角色类型：人形->Auto（自带动画状态机），四足：Custom（手动播动画），由编辑器自动切换。
- 删除动画姿态（二级姿态）属性`animationStance`，动画姿态（二级姿态）从角色中提出，采用【SubStance】对象承接原来的接口。`animationStance`替换为`currentSubStance.assetId`。原先角色修改动画姿态ID会使角色播放该姿态，替换为`currentSubStance.play`和`currentSubStance.play`。原先角色修改动画姿态ID为空会停止播放姿态，替换为`currentSubStance.stop`。删除加载动画姿态（二级姿态）方法`loadStance`，替换为`loadSubStance`。删除停止当前动画姿态（二级姿态）方法`stopStance`，替换为`currentSubStance.stop`。
- 删除基础姿态属性`basicStance`，基础姿态从角色中提出，采用【Stance】对象承接原来的接口。`basicStance`替换为`currentStance.assetId`。原先角色修改基础姿态ID会使角色播放该姿态，替换为`currentStance.play`和`currentStance.play`。原先角色修改基础姿态ID为空会停止播放姿态，替换为`currentStance.stop`。
- 删除角色外观对象属性`appearance`，替换为角色描述`description`。角色外观由原来的层级对象修改为配置表形式，统一使用【CharacterDescription】描述。设置/获取外观方法`getAppearance`和`setAppearance`替换为`getDescription`和`setDescription`。清空角色外观方法`clearAppearance`替换为`clearDescription`，其中参数设置为`appearance: true`。此外删除角色外观设置权限属性`canSetAppearanceData`。
- 删除清空角色挂件方法`clearDecorations`，替换为`clearDescription`，其中参数设置为`slotAndDecoration: true`。删除清空单个挂件方法`clearOneDecoration`、删除获取角色挂件对象列表方法`getDecorations`，两者都需要使用【CharacterDescription】中的挂件配置项，去访问具体插槽挂件数组`description.advance.slotAndDecoration.slot["SlotName"].decoration`，前者可以调用delete方法删除挂件，后者可以访问数组中的元素。删除加载插槽数据方法`loadSlotAndEditorDataByGuid`替换为`setDescription`。此外删除加载挂件方法`loadDecoration`和`loadSlotAndEditorDataByPath`。
- 删除角色外观分类属性`appearanceType`，替换为角色类型`characterType`。
- 删除角色阴影控制属性：`baseShadowLocationOffset`、`baseShadowMaxVisibleHeight`和`baseShadowScale`。后续角色阴影由编辑器底层控制。
- 删除角色碰撞相关接口：`capsuleRadius`和`capsuleHalfHeight`。由于后续角色碰撞体支持多种形状，碰撞范围接口替换为`collisionExtent`。删除角色碰撞开关`collisionEnable`，替换为`setCollision`和`getCollision`方法。此外`collisionWithOtherCharacterEnable`替换为`collisionWithOtherCharacterEnabled`。删除角色胶囊体矫正属性`usedCapsuleCorrection`，替换为`capsuleCorrectionEnabled`。
- 角色头顶显示名称`characterName`替换为`displayName`。头顶UI获取方法`getHeadUIWidget`替换为只读属性`overheadUI`。此外原先成员属性`headUIVisible`和`headUIVisibleRange`由于功能是作用于场景中全部对象，所以删除后替换为静态属性`nameVisible`和`nameDisplayDistance`。
- 角色是否可被站立属性`canStepUpOn`替换为`canStandOn`。
- 角色下蹲能力开关`crouchEnable`替换为`crouchEnabled`；角色移动能力开关`moveEnable`替换为`moveEnabled`;角色跳跃能力开关`jumpEnable`替换为`jumpEnabled`;角色跳跃出水能力开关`jumpingOutOfWaterEnable`替换为`canJumpOutOfWater`；角色布娃娃开关`ragdollEnable`替换为`ragdollEnabled`。角色摩擦单独生效属性`separateBrakingFrictionEnable`，替换为`groundFrictionEnabled`，此处需注意原先属性值false对应新属性值true，原先属性值true对应新属性值false。
- 删除角色本地可见性属性`locallyVisible`，后续在客户端使用`setVisibility`和`getVisibility`替换。删除角色同步可见性属性`visible`，后续在服务端使用`setVisibility`和`getVisibility`替换。删除角色本地可见性接口`setLocallyVisibility`，在客户端使用`setVisibility`替换。
- 角色移动模式属性`movementState`替换为`movementMode`，同时移动模式变化委托`onMovementStateChanged`替换为`onMovementModeChange`。
- 角色大小属性`scale`替换为角色持有的【worldTransform】中`scale`属性。
- 删除角色换装完成委托`onLoadAppearanceDataAllCompleted`、`onLoadDecorationsAllCompleted`、`onSetAppearanceDataCompleted`。三者合并为角色描述加载完成委托`onDescriptionComplete`。删除角色外观变化委托`onMeshChanged`、`onTextureChanged`。两者合并为角色描述变化完成委托`onDescriptionChange`，其中传出操作码标识发生变化的属性。此外角色外观异步等待函数`appearanceReady`替换为【GameObject】中`asyncReady`基类异步方法完成。
- 【Character】对象继承【Pawn】对象新增`player`属性，玩家角色`player`为控制玩家，NPC`player`为空。
- 删除增加角色移动输入方法`addMoveInput`，替换为`addMovement`。
- 删除角色`attach`方法，将对象插入角色插槽使用attachToSlot替换。同时新增`detachFromSlot`和`detachAllFromSlot`方法卸载角色插槽挂载的对象。
- 删除获取玩家控制器旋转方法`getControlRotator`，替换为【Player】对象中的静态方法`getControllerRotation`。同时新增`setControllerRotation`方法搭配使用。
- 删除获取角色插槽名称方法`getSlotName`，通过TS语法可以直接在枚举名称和值之间转化。
- 删除角色播放动画方法`playAnimation`和`playAnimationLocally`，替换为`loadAnimation().play()`分别在服务端调用和客户端调用。删除角色判断动画播放状态接口`isPlayingAnimation`，替换为`currentAnimation.isPlaying`。删除角色停止动画接口`stopAnimation`，替换为`currentAnimation.stop`。
- 删除角色上浮/下潜接口`swimmingDown`和`swimmingUp`，分别替换为`swimDown`和`swimUp`。

| 接口名称（老）                    | 属类（老）    | 接口类型（老）      | 接口名称（新）                                               | 属类（新） | 修改方案 |
| --------------------------------- | ------------- | ------------------- | ------------------------------------------------------------ | ---------- | -------- |
| airControl                        | CharacterBase | GetAccessor         | driftControl                                                 | Character  |          |
| airControlBoostMultiplier         | CharacterBase | GetAccessor         | -                                                            |            |          |
| airControlBoostVelocityThreshold  | CharacterBase | GetAccessor         | -                                                            |            |          |
| animationMode                     | CharacterBase | GetAccessor         | -                                                            |            |          |
| animationStance                   | CharacterBase | GetAccessor         | currentSubStance.assetId                                     | SubStance  |          |
| appearance                        | CharacterBase | GetAccessor         | description                                                  | Character  |          |
| appearanceType                    | CharacterBase | GetAccessor         | characterType                                                | Character  |          |
| baseShadowLocationOffset          | CharacterBase | GetAccessor         | -                                                            |            |          |
| baseShadowMaxVisibleHeight        | CharacterBase | GetAccessor         | -                                                            |            |          |
| baseShadowScale                   | CharacterBase | GetAccessor         | -                                                            |            |          |
| basicStance                       | CharacterBase | GetAccessor         | currentStance.assetId                                        | Stance     |          |
| basicStanceAimOffsetEnable        | CharacterBase | GetAccessor         | currentStance.aimOffsetEnabled                               | Stance     |          |
| brakingDecelerationFalling        | CharacterBase | GetAccessor         | horizontalBrakingDecelerationFalling                         | Character  |          |
| canSetAppearanceData              | CharacterBase | GetAccessor         | -                                                            |            |          |
| canStepUpOn                       | CharacterBase | GetAccessor         | canStandOn                                                   | Character  |          |
| capsuleHalfHeight                 | CharacterBase | GetAccessor         | collisionExtent.z                                            | Vector     |          |
| capsuleRadius                     | CharacterBase | GetAccessor         | Max(collisionExtent.x, collisionExtent.y)                    | Vector     |          |
| characterName                     | CharacterBase | GetAccessor         | displayName                                                  | Character  |          |
| collisionEnable                   | CharacterBase | GetAccessor         | setCollision                                                 | Character  |          |
| collisionWithOtherCharacterEnable | CharacterBase | GetAccessor         | collisionWithOtherCharacterEnabled                           | Character  |          |
| crouchEnable                      | CharacterBase | GetAccessor         | crouchEnabled                                                | Character  |          |
| headUIVisible                     | CharacterBase | GetAccessor         | nameVisible                                                  | Character  |          |
| headUIVisibleRange                | CharacterBase | GetAccessor         | nameDisplayDistance                                          | Character  |          |
| jumpEnable                        | CharacterBase | GetAccessor         | jumpEnabled                                                  | Character  |          |
| jumpingOutOfWaterEnable           | CharacterBase | GetAccessor         | canJumpOutOfWater                                            | Character  |          |
| locallyVisible                    | CharacterBase | GetAccessor         | setVisibility                                                | GameObject |          |
| moveEnable                        | CharacterBase | GetAccessor         | movementEnabled                                              | Character  |          |
| movementState                     | CharacterBase | GetAccessor         | movementMode                                                 | Character  |          |
| outOfWaterZ                       | CharacterBase | GetAccessor         | outOfWaterVerticalSpeed                                      | Character  |          |
| ragdollEnable                     | CharacterBase | GetAccessor         | ragdollEnabled                                               | Character  |          |
| scale                             | CharacterBase | GetAccessor         | worldTransform.scale                                         | Transform  |          |
| separateBrakingFrictionEnable     | CharacterBase | GetAccessor         | groundFrictionEnabled                                        | Character  |          |
| usedCapsuleCorrection             | CharacterBase | GetAccessor         | capsuleCorrectionEnabled                                     | Character  |          |
| visible                           | CharacterBase | GetAccessor         | getVisibility                                                | GameObject |          |
| airControl                        | CharacterBase | SetAccessor         | driftControl                                                 | Character  |          |
| airControlBoostMultiplier         | CharacterBase | SetAccessor         | -                                                            |            |          |
| airControlBoostVelocityThreshold  | CharacterBase | SetAccessor         | -                                                            |            |          |
| animationMode                     | CharacterBase | SetAccessor         | -                                                            |            |          |
| animationStance                   | CharacterBase | SetAccessor         | currentSubStance.assetId                                     | SubStance  |          |
| appearanceType                    | CharacterBase | SetAccessor         | characterType                                                | Character  |          |
| baseShadowLocationOffset          | CharacterBase | SetAccessor         | -                                                            |            |          |
| baseShadowMaxVisibleHeight        | CharacterBase | SetAccessor         | -                                                            |            |          |
| baseShadowScale                   | CharacterBase | SetAccessor         | -                                                            |            |          |
| basicStance                       | CharacterBase | SetAccessor         | currentStance.assetId                                        | Stance     |          |
| basicStanceAimOffsetEnable        | CharacterBase | SetAccessor         | currentStance.aimOffsetEnabled                               | Stance     |          |
| brakingDecelerationFalling        | CharacterBase | SetAccessor         | horizontalBrakingDecelerationFalling                         | Character  |          |
| canStepUpOn                       | CharacterBase | SetAccessor         | canStandOn                                                   | Character  |          |
| capsuleHalfHeight                 | CharacterBase | SetAccessor         | collisionExtent.z                                            | Vector     |          |
| capsuleRadius                     | CharacterBase | SetAccessor         | Max(collisionExtent.x, collisionExtent.y)                    | Vector     |          |
| characterName                     | CharacterBase | SetAccessor         | displayName                                                  | Character  |          |
| collisionEnable                   | CharacterBase | SetAccessor         | setCollision                                                 | Character  |          |
| collisionWithOtherCharacterEnable | CharacterBase | SetAccessor         | collisionWithOtherCharacterEnabled                           | Character  |          |
| crouchEnable                      | CharacterBase | SetAccessor         | crouchEnabled                                                | Character  |          |
| headUIVisible                     | CharacterBase | SetAccessor         | nameVisible                                                  | Character  |          |
| headUIVisibleRange                | CharacterBase | SetAccessor         | nameDisplayDistance                                          | Character  |          |
| jumpEnable                        | CharacterBase | SetAccessor         | jumpEnabled                                                  | Character  |          |
| jumpingOutOfWaterEnable           | CharacterBase | SetAccessor         | canJumpOutOfWater                                            | Character  |          |
| locallyVisible                    | CharacterBase | SetAccessor         | setVisibility                                                | GameObject |          |
| moveEnable                        | CharacterBase | SetAccessor         | movementEnabled                                              | Character  |          |
| outOfWaterZ                       | CharacterBase | SetAccessor         | outOfWaterVerticalSpeed                                      | Character  |          |
| ragdollEnable                     | CharacterBase | SetAccessor         | ragdollEnabled                                               | Character  |          |
| scale                             | CharacterBase | SetAccessor         | worldTransform.scale                                         | Transform  |          |
| separateBrakingFrictionEnable     | CharacterBase | SetAccessor         | groundFrictionEnabled                                        | Character  |          |
| usedCapsuleCorrection             | CharacterBase | SetAccessor         | capsuleCorrectionEnabled                                     | Character  |          |
| visible                           | CharacterBase | SetAccessor         | getVisibility                                                | GameObject |          |
| onLoadAppearanceDataAllCompleted  | CharacterBase | PropertyDeclaration | onDescriptionComplete                                        | Character  |          |
| onLoadDecorationsAllCompleted     | CharacterBase | PropertyDeclaration | onDescriptionComplete                                        | Character  |          |
| onMeshChanged                     | CharacterBase | PropertyDeclaration | onDescriptionChange                                          | Character  |          |
| onMovementStateChanged            | CharacterBase | PropertyDeclaration | onMovementModeChange                                         | Character  |          |
| onSetAppearanceDataCompleted      | CharacterBase | PropertyDeclaration | onDescriptionComplete                                        | Character  |          |
| onTextureChanged                  | CharacterBase | PropertyDeclaration | onDescriptionChange                                          | Character  |          |
| player                            | CharacterBase | PropertyDeclaration | player                                                       | Pawn       |          |
| addMoveInput                      | CharacterBase | MethodDeclaration   | addMovement                                                  | Character  |          |
| appearanceReady                   | CharacterBase | MethodDeclaration   | asyncReady                                                   | GameObject |          |
| attach                            | CharacterBase | MethodDeclaration   | attachToSlot                                                 | Character  |          |
| clearAppearance                   | CharacterBase | MethodDeclaration   | clearDescription                                             | Character  |          |
| clearDecorations                  | CharacterBase | MethodDeclaration   | clearDescription                                             | Character  |          |
| clearOneDecoration                | CharacterBase | MethodDeclaration   | description.advance.slotAndDecoration.slot["SlotName"].decoration.delete() | Character  |          |
| detachFromGameObject              | CharacterBase | MethodDeclaration   | parent                                                       | GameObject |          |
| getAppearance                     | CharacterBase | MethodDeclaration   | getDescription                                               | Character  |          |
| getControlRotator                 | CharacterBase | MethodDeclaration   | getControllerRotation                                        | Player     |          |
| getDecorations                    | CharacterBase | MethodDeclaration   | -                                                            |            |          |
| getHeadUIWidget                   | CharacterBase | MethodDeclaration   | overheadUI                                                   | Character  |          |
| getSlotName                       | CharacterBase | MethodDeclaration   | -                                                            |            |          |
| isPlayingAnimation                | CharacterBase | MethodDeclaration   | currentAnimation.isPlaying                                   | Animation  |          |
| loadDecoration                    | CharacterBase | MethodDeclaration   | setDescription                                               | Character  |          |
| loadSlotAndEditorDataByGuid       | CharacterBase | MethodDeclaration   | setDescription                                               | Character  |          |
| loadSlotAndEditorDataByPath       | CharacterBase | MethodDeclaration   | -                                                            |            |          |
| loadStance                        | CharacterBase | MethodDeclaration   | loadSubStance                                                | Character  |          |
| playAnimation                     | CharacterBase | MethodDeclaration   | loadAnimation().play()                                       | Animation  |          |
| playAnimationLocally              | CharacterBase | MethodDeclaration   | loadAnimation().play()                                       | Animation  |          |
| setAppearance                     | CharacterBase | MethodDeclaration   | setDescription                                               | Character  |          |
| setLocallyVisibility              | CharacterBase | MethodDeclaration   | setVisibility                                                | GameObject |          |
| stopAnimation                     | CharacterBase | MethodDeclaration   | currentAnimation.stop()                                      | Animation  |          |
| stopStance                        | CharacterBase | MethodDeclaration   | currentSubStance.stop()                                      | SubStance  |          |
| swimmingDown                      | CharacterBase | MethodDeclaration   | swimDown                                                     | Character  |          |
| swimmingUp                        | CharacterBase | MethodDeclaration   | swimUp                                                       | Character  |          |

#### [修改]`<类>`ChatService 聊天服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。

| 接口名称（老） | 属类（老）  | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ----------- | ----------------- | -------------- | ---------- | -------- |
| getInstance    | ChatService | MethodDeclaration | -              |            |          |

#### [修改]`<namespace>`Core 函数

| 枚举名称（老） | 枚举名称（新） | 修改方案 |
| -------------- | -------------- | -------- |
| Class          | Component      | 替换     |
| Function       | RemoteFunction | 替换     |
| Type           | Serializable   | 替换     |

#### [修改]`<namespace>`DataStorage 数据存储

- 命名空间【DataStorage】改为类【DataStorage】。
- 数据存储统一使用一套接口，不再区分玩家数据和自定义数据。之前使用含player的玩家数据接口，key值统一为Player的userId。027版本使用新接口，传入userId作为key的参数即可获取之前的数据。

`asyncGetPlayerData`和`asyncGetCustomData`合并，->`asyncGetData`。

`asyncSetPlayerData`和`asyncSetCustomData`合并，->`asyncSetData`。

asyncRemoveCustomData和`asyncRemovePlayerData`合并，->`asyncRemoveData`。

| 接口名称（老）        | 属类（老） | 接口类型（老）      | 接口名称（新）        | 属类（新）  | 修改方案 |
| --------------------- | ---------- | ------------------- | --------------------- | ----------- | -------- |
| asyncGetCustomData    | -          | FunctionDeclaration | asyncGetData          | DataStorage | 替换     |
| asyncGetOtherGameData | -          | FunctionDeclaration | asyncGetOtherGameData | DataStorage | 替换     |
| asyncGetPlayerData    | -          | FunctionDeclaration | -                     |             |          |
| asyncRemoveCustomData | -          | FunctionDeclaration | asyncRemoveData       | DataStorage | 替换     |
| asyncRemovePlayerData | -          | FunctionDeclaration | -                     |             |          |
| asyncSetCustomData    | -          | FunctionDeclaration | asyncSetData          | DataStorage | 替换     |
| asyncSetOtherGameData | -          | FunctionDeclaration | asyncSetOtherGameData | DataStorage | 替换     |
| asyncSetPlayerData    | -          | FunctionDeclaration | -                     |             |          |
| setTemporaryStorage   | -          | FunctionDeclaration | setTemporaryStorage   | DataStorage | 替换     |
| sizeOfData            | -          | FunctionDeclaration | getDataSize           | DataStorage | 替换     |

#### [修改]`<类>`DataCenterC 数据中心客户端

- 去除getInstance，缩短调用链。

| 接口名称（老） | 属类（老）  | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ----------- | ----------------- | -------------- | ---------- | -------- |
| getInstance    | DataCenterC | MethodDeclaration | -              |            |          |

#### [修改]`<类>`DataCenterS 数据中心服务端

- 去除getInstance，缩短调用链。
- `onPlayerJoined`和`onPlayerLeft`事件委托替换为`onPlayerJoin`和`onPlayerLeave`，去除时态。

| 接口名称（老） | 属类（老）  | 接口类型（老）      | 接口名称（新） | 属类（新）  | 修改方案 |
| -------------- | ----------- | ------------------- | -------------- | ----------- | -------- |
| getInstance    | DataCenterS | MethodDeclaration   | -              |             |          |
| onPlayerJoined | DataCenterS | PropertyDeclaration | onPlayerJoin   | DataCenterS |          |
| onPlayerLeft   | DataCenterS | PropertyDeclaration | onPlayerLeave  | DataCenterS |          |

#### [修改]`<类>`DebugService 聊天服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。

| 接口名称（老） | 属类（老）   | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ------------ | ----------------- | -------------- | ---------- | -------- |
| getInstance    | DebugService | MethodDeclaration | -              |            |          |

#### [删除]`<类>`Decoration 挂件数据

- 删除【Decoration】挂件数据类，后续统一使用【CharacterDescription】挂件数据对象。

#### [删除]`<类>`DirectionalLight 平行光

- 删除【DirectionalLight】平行光，接口修改为静态调用形式迁移至新增对象【Lighting】。
- 删除继承自【GameObject】的接口，【Lighting】不继承【GameObject】。
- 删除光强`intensity`，替换为`brightness`。
- 删除无效属性`baseShadowEnable`。
- 删除阴影投影`castShadowsEnable`，替换为`castShadowsEnabled`。
- 删除色温`temperature`，替换为`temperatureEnabled`。

| 接口名称（老）    | 属类（老）       | 接口类型（老） | 接口名称（新）     | 属类（新） | 修改方案 |
| ----------------- | ---------------- | -------------- | ------------------ | ---------- | -------- |
| baseShadowEnable  | DirectionalLight | GetAccessor    | -                  |            |          |
| castShadowsEnable | DirectionalLight | GetAccessor    | castShadowsEnabled | Lighting   |          |
| intensity         | DirectionalLight | GetAccessor    | brightness         | Lighting   |          |
| lightColor        | DirectionalLight | GetAccessor    | lightColor         | Lighting   |          |
| pitchAngle        | DirectionalLight | GetAccessor    | pitchAngle         | Lighting   |          |
| temperature       | DirectionalLight | GetAccessor    | temperature        | Lighting   |          |
| temperatureEnable | DirectionalLight | GetAccessor    | temperatureEnabled | Lighting   |          |
| yawAngle          | DirectionalLight | GetAccessor    | yawAngle           | Lighting   |          |
| baseShadowEnable  | DirectionalLight | SetAccessor    | -                  |            |          |
| castShadowsEnable | DirectionalLight | SetAccessor    | castShadowsEnabled | Lighting   |          |
| intensity         | DirectionalLight | SetAccessor    | brightness         | Lighting   |          |
| lightColor        | DirectionalLight | SetAccessor    | lightColor         | Lighting   |          |
| pitchAngle        | DirectionalLight | SetAccessor    | pitchAngle         | Lighting   |          |
| temperature       | DirectionalLight | SetAccessor    | temperature        | Lighting   |          |
| temperatureEnable | DirectionalLight | SetAccessor    | temperatureEnabled | Lighting   |          |
| yawAngle          | DirectionalLight | SetAccessor    | yawAngle           | Lighting   |          |

#### [删除]`<类>`EffectLogical 区域效果

- 删除低频使用对象【EffectLogical】

#### [修改]`<类>`EffectService 特效服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。成员函数全部封装成静态方法。
- 删除`clearAll()`方法，屏蔽用户对内部对象池的操作带来的风险。
- 新增`playOnGameObject`方法，参数使用联合类型来实现`playEffectOnPlayer`和`playEffectOnGameObject`两个方法的功能。

| 接口名称（老）         | 属类（老）    | 接口类型（老）    | 接口名称（新）   | 属类（新）    | 修改方案 |
| ---------------------- | ------------- | ----------------- | ---------------- | ------------- | -------- |
| clearAll               | EffectService | MethodDeclaration | -                |               |          |
| getEffectGameObject    | EffectService | MethodDeclaration | getEffectById    | EffectService |          |
| getInstance            | EffectService | MethodDeclaration | -                |               |          |
| playEffectAtLocation   | EffectService | MethodDeclaration | playAtPosition   | EffectService |          |
| playEffectOnGameObject | EffectService | MethodDeclaration | playOnGameObject | EffectService |          |
| playEffectOnPlayer     | EffectService | MethodDeclaration | -                |               |          |
| stopAllEffect          | EffectService | MethodDeclaration | stopAll          | EffectService |          |
| stopEffect             | EffectService | MethodDeclaration | stop             | EffectService |          |

#### [删除]`<类>`Equipment 装备

- 删除低频使用对象【Equipment】

#### [修改]`<命名空间>`Events 函数

- 新增【Event】类，静态封装命名空间【Events】中与事件相关的方法。
- `dispatchLocal`方法替换为`dispatchToLocal`，其余事件通信接口`addClientListener`、`addLocalListener`、`addServerListener`、`dispatchToAllClient`、`dispatchToClient`和`dispatchToServer`封装为类静态方法。
- 删除应用状态监听方法：`addOnResumeListener`、`addOnPauseListener`和`addExitListener`。在【SystemUtil】类中新增onResume、onPause和onExit事件委托进行替换，回调方法统一采用委托对象绑定函数的形式。
- 删除聚焦状态监听方法：`addFocusListener`和`addUnfocusedListener`。在【WindowUtil】类中新增onFocus和onDefocus事件委托进行替换，回调方法统一采用委托对象绑定函数的形式。
- 删除玩家进入/离开游戏监听方法：`addPlayerJoinedListener`和`addPlayerLeftListener`。在【Player】类中新增onPlayerJoin和onPlayerLeave事件委托进行替换，回调方法统一采用委托对象绑定函数的形式。

| 接口名称（老）          | 属类（老） | 接口类型（老）      | 接口名称（新）      | 属类（新） | 修改方案 |
| ----------------------- | ---------- | ------------------- | ------------------- | ---------- | -------- |
| addClientListener       | -          | FunctionDeclaration | addClientListener   | Event      | 替换     |
| addExitListener         | -          | FunctionDeclaration | onExit              | SystemUtil |          |
| addFocusListener        | -          | FunctionDeclaration | onFocus             | WindowUtil |          |
| addLocalListener        | -          | FunctionDeclaration | addLocalListener    | Event      |          |
| addOnPauseListener      | -          | FunctionDeclaration | onPause             | SystemUtil |          |
| addOnResumeListener     | -          | FunctionDeclaration | onResume            | SystemUtil |          |
| addPlayerJoinedListener | -          | FunctionDeclaration | onPlayerJoin        | Player     |          |
| addPlayerLeftListener   | -          | FunctionDeclaration | onPlayerLeave       | Player     |          |
| addServerListener       | -          | FunctionDeclaration | addServerListener   | Event      |          |
| addUnfocusedListener    | -          | FunctionDeclaration | onDefocus           | WindowUtil |          |
| dispatchLocal           | -          | FunctionDeclaration | dispatchToLocal     | Event      |          |
| dispatchToAllClient     | -          | FunctionDeclaration | dispatchToAllClient | Event      |          |
| dispatchToClient        | -          | FunctionDeclaration | dispatchToClient    | Event      |          |
| dispatchToServer        | -          | FunctionDeclaration | dispatchToServer    | Event      |          |

#### [删除]`<类>`ExponentialHeightFog 环境雾

- **删除【ExponentialHeightFog】环境雾，接口修改为静态调用形式迁移至新增对象【Fog】**。因为环境雾并不支持叠加使用，场景中有且仅有一份生效，故027版本将**其放置到世界对象**以默认配置存在。
- 删除继承自【GameObject】的接口，**【Fog】不继承【GameObject】**。
- 删除属性中的fog前缀：`fogDensity`-> `density`；`fogEnable`-> `enabled`；`fogHeight`-> `height`；`fogHeightFalloff`-> `heightFalloff`；`fogInscatteringColor`-> `inscatteringColor`；`fogMaxOpacity`-> `maxOpacity`；
- 删除设置雾气预设方法`setPresetByIndex`，替换为`setPreset`。

| 接口名称（老）       | 属类（老）           | 接口类型（老）    | 接口名称（新）    | 属类（新） | 修改方案 |
| -------------------- | -------------------- | ----------------- | ----------------- | ---------- | -------- |
| fogDensity           | ExponentialHeightFog | GetAccessor       | density           | Fog        |          |
| fogEnable            | ExponentialHeightFog | GetAccessor       | enabled           | Fog        |          |
| fogHeight            | ExponentialHeightFog | GetAccessor       | height            | Fog        |          |
| fogHeightFalloff     | ExponentialHeightFog | GetAccessor       | heightFalloff     | Fog        |          |
| fogInscatteringColor | ExponentialHeightFog | GetAccessor       | inscatteringColor | Fog        |          |
| fogMaxOpacity        | ExponentialHeightFog | GetAccessor       | maxOpacity        | Fog        |          |
| fogDensity           | ExponentialHeightFog | SetAccessor       | density           | Fog        |          |
| fogEnable            | ExponentialHeightFog | SetAccessor       | enabled           | Fog        |          |
| fogHeight            | ExponentialHeightFog | SetAccessor       | height            | Fog        |          |
| fogHeightFalloff     | ExponentialHeightFog | SetAccessor       | heightFalloff     | Fog        |          |
| fogInscatteringColor | ExponentialHeightFog | SetAccessor       | inscatteringColor | Fog        |          |
| fogMaxOpacity        | ExponentialHeightFog | SetAccessor       | maxOpacity        | Fog        |          |
| setPresetByIndex     | ExponentialHeightFog | MethodDeclaration | setPreset         | Fog        |          |

#### [删除]`<类>`FourFootStandard 四足外观

- 删除【SomatotypeBase】四足角色外观对象。统一使用【CharacterDescription】角色外观描述对象中的`base`对象进行四足角色的外观操作。
- 删除设置/获取四足体型方法`getSomatotype`和`changeSomatotype`，后续四足外观只支持设置全身模型。

| 接口名称（老）   | 属类（老）       | 接口类型（老）    | 接口名称（新） | 属类（新）            | 修改方案 |
| ---------------- | ---------------- | ----------------- | -------------- | --------------------- | -------- |
| changeSomatotype | FourFootStandard | MethodDeclaration | -              |                       |          |
| getSomatotype    | FourFootStandard | MethodDeclaration | -              |                       |          |
| getWholeBody     | FourFootStandard | MethodDeclaration | base.wholeBody | CharacterDescriptioon |          |
| setWholeBody     | FourFootStandard | MethodDeclaration | base.wholeBody | CharacterDescriptioon |          |

#### [修改]`<类>`GameObject 游戏对象

- 统一使用世界变换`worldTransform`和本地变换`localTransform`承载原先对象内Transform相关的属性和方法。每个【GameObject】持有`worldTransform`和`localTransform`。两个transform对象保存了【GameObject】的引用，所有针对`worldTransform`和`localTransform`的修改都会反应到对象身上。

```TypeScript
// a是一个GameObject对象
a: GameObject;

// worldTransform引用了对象a，对worldTransform的操作会反应在对象a上。
let transform = a.worldTransform；
transform.position = new Vector(100, 0, 0);

// position没有保存对象的引用，对position 的操作并不会影响对象a。
let position = a.worldTransform.position；
```

- 删除无效属性：`staticStatus`、`lockStatus`。
- 删除内部方法：`attachComponent`和`detachComponent`。
- 对象生成接口`spawn`和`asyncSpawn`方法将assetId作为必填参数放在首位，即spawn时第一个参数必须传入对象的资源ID。而其他对象生成所需用到的属性，则统一通过GameObjectInfo对象作为可选参数进行设置。此外删除废弃方法：`asyncSpawnGameObject`和`spawnGameObject`。
- 新增`isReady`属性标明对象的准备状态。原先的`ready`方法改为`asyncReady`。
- 删除`visible`属性，统一使用`getVisibility`方法替换。
- 删除内部私有接口：`scriptNumberPropPathMap`和 `scriptPropPathNumberMap`。
- 删除`addDestroyCallback`和`deleteDestroyCallback`方法，使用`onDestroyDelegate`委托替换，回调方法统一采用委托对象绑定函数的形式。
- 删除内部方法：`attachComponent`和`detachComponent`。
- 删除对象挂载/卸载方法：`attachToGameObject`和`detachFromGameObject`，使用`parent`属性来控制对象的挂载和卸载，便于统一维护挂载和父子两对关系。
- `find`方法改名为`findGameObjectById`，强调通过`gameObjectId`查找对象。
- 查询方法返回的是对象数组（复数），`findGameObjectByTag`替换为`findGameObjectsByTag`。
- 空间大小统一使用extent命名，`getBoundingBoxSize`替换为`getBoundingBoxExtent`。
- 将“碰撞”属性从【GameObject】迁移，废弃原有碰撞相关方法`getCollision`和`setCollision`，配合碰撞枚举中“跟随父节点”的去除，以简化世界中的碰撞机制。目前游戏对象中仅【Model】和【Character】保留碰撞接口。

| 接口名称（老）          | 属类（老） | 接口类型（老）      | 接口名称（新）                  | 属类（新） | 修改方案 |
| ----------------------- | ---------- | ------------------- | ------------------------------- | ---------- | -------- |
| forwardVector           | GameObject | GetAccessor         | worldTransform.forwardVector    | GameObject |          |
| lockStatus              | GameObject | GetAccessor         | -                               |            |          |
| relativeLocation        | GameObject | GetAccessor         | localTransform.position          | GameObject |          |
| relativeRotation        | GameObject | GetAccessor         | localTransform.rotation         | GameObject |          |
| relativeScale           | GameObject | GetAccessor         | localTransform.scale            | GameObject |          |
| rightVector             | GameObject | GetAccessor         | worldTransform.rightVector      | GameObject |          |
| staticStatus            | GameObject | GetAccessor         | -                               |            |          |
| transform               | GameObject | GetAccessor         | worldTransform                  | GameObject |          |
| upVector                | GameObject | GetAccessor         | worldTransform.upVector         | GameObject |          |
| useUpdate               | GameObject | GetAccessor         | -                               |            |          |
| visible                 | GameObject | GetAccessor         | getVisibility                   | GameObject |          |
| worldLocation           | GameObject | GetAccessor         | worldTransform.position          | GameObject |          |
| worldRotation           | GameObject | GetAccessor         | worldTransform.rotation         | GameObject |          |
| worldScale              | GameObject | GetAccessor         | worldTransform.scale            | GameObject |          |
| lockStatus              | GameObject | SetAccessor         | -                               |            | 删除     |
| relativeLocation        | GameObject | SetAccessor         | localTransform.position         | GameObject |          |
| relativeRotation        | GameObject | SetAccessor         | localTransform.rotation         | GameObject |          |
| relativeScale           | GameObject | SetAccessor         | localTransform.scale            | GameObject |          |
| transform               | GameObject | SetAccessor         | worldTransform                  | GameObject |          |
| useUpdate               | GameObject | SetAccessor         | -                               |            |          |
| worldLocation           | GameObject | SetAccessor         | worldTransform.position         | GameObject |          |
| worldRotation           | GameObject | SetAccessor         | worldTransform.rotation         | GameObject |          |
| worldScale              | GameObject | SetAccessor         | worldTransform.scale            | GameObject |          |
| scriptNumberPropPathMap | GameObject | PropertyDeclaration | -                               |            |          |
| scriptPropPathNumberMap | GameObject | PropertyDeclaration | -                               |            |          |
| addDestroyCallback      | GameObject | MethodDeclaration   | onDestroyDelegate               | GameObject |          |
| asyncFind               | GameObject | MethodDeclaration   | asyncFindGameObjectById         | GameObject |          |
| asyncGetScriptByName    | GameObject | MethodDeclaration   | getScriptByName                 | GameObject |          |
| asyncSpawn              | GameObject | MethodDeclaration   | asyncSpawn                      | GameObject |          |
| asyncSpawnGameObject    | GameObject | MethodDeclaration   | asyncSpawn                      | GameObject |          |
| attachComponent         | GameObject | MethodDeclaration   | -                               |            |          |
| attachToGameObject      | GameObject | MethodDeclaration   | parent                          | GameObject |          |
| deleteDestroyCallback   | GameObject | MethodDeclaration   | onDestroyDelegate.remove        | GameObject |          |
| detachComponent         | GameObject | MethodDeclaration   | -                               |            |          |
| detachFromGameObject    | GameObject | MethodDeclaration   | parent                          | GameObject |          |
| find                    | GameObject | MethodDeclaration   | findGameObjectById              | GameObject |          |
| findGameObjectByTag     | GameObject | MethodDeclaration   | findGameObjectsByTag            | GameObject |          |
| getBoundingBoxSize      | GameObject | MethodDeclaration   | getBoundingBoxExtent            | GameObject |          |
| getChildByGuid          | GameObject | MethodDeclaration   | getChildByGameObjectId          | GameObject |          |
| getChildrenBoxCenter    | GameObject | MethodDeclaration   | getChildrenBoundingBoxCenter    | GameObject |          |
| getCollision            | GameObject | MethodDeclaration   | -                               |            |          |
| getForwardVector        | GameObject | MethodDeclaration   | worldTransform.getForwardVector | GameObject |          |
| getGameObjectByName     | GameObject | MethodDeclaration   | findGameObjectByName            | GameObject |          |
| getGameObjectsByName    | GameObject | MethodDeclaration   | findGameObjectsByName           | GameObject |          |
| getRelativeLocation     | GameObject | MethodDeclaration   | localTransform.position         | GameObject |          |
| getRelativeRotation     | GameObject | MethodDeclaration   | localTransform.rotation         | GameObject |          |
| getRelativeScale        | GameObject | MethodDeclaration   | localTransform.scale            | GameObject |          |
| getRightVector          | GameObject | MethodDeclaration   | worldTransform.getRightVector   | GameObject |          |
| getScriptByGuid         | GameObject | MethodDeclaration   | getScript                       | GameObject |          |
| getSourceAssetGuid      | GameObject | MethodDeclaration   | assetId                         | GameObject |          |
| getTransform            | GameObject | MethodDeclaration   | worldTransform                  | GameObject |          |
| getUpVector             | GameObject | MethodDeclaration   | worldTransform.getUpVector      | GameObject |          |
| getVisibility           | GameObject | MethodDeclaration   | getVisibility                   | GameObject |          |
| getWorldLocation        | GameObject | MethodDeclaration   | worldTransform.position         | GameObject |          |
| getWorldRotation        | GameObject | MethodDeclaration   | worldTransform.rotation         | GameObject |          |
| getWorldScale           | GameObject | MethodDeclaration   | worldTransform.scale            | GameObject |          |
| ready                   | GameObject | MethodDeclaration   | asyncReady                      | GameObject |          |
| setCollision            | GameObject | MethodDeclaration   | -                               |            |          |
| setLocationAndRotation  | GameObject | MethodDeclaration   | -                               |            |          |
| setRelativeLocation     | GameObject | MethodDeclaration   | localTransform.position         | GameObject |          |
| setRelativeRotation     | GameObject | MethodDeclaration   | localTransform.rotation         | GameObject |          |
| setRelativeScale        | GameObject | MethodDeclaration   | localTransform.scale            | GameObject |          |
| setTransform            | GameObject | MethodDeclaration   | worldTransform                  | GameObject |          |
| setWorldLocation        | GameObject | MethodDeclaration   | worldTransform.position         | GameObject |          |
| setWorldRotation        | GameObject | MethodDeclaration   | worldTransform.rotation         | GameObject |          |
| setWorldScale           | GameObject | MethodDeclaration   | worldTransform.scale            | GameObject |          |
| spawn                   | GameObject | MethodDeclaration   | spawn                           | GameObject |          |
| spawnGameObject         | GameObject | MethodDeclaration   | spawn                           | GameObject |          |

#### [修改]`<命名空间>`Gameplay函数

| 接口名称（老）                | 接口名称（新）                | 属类（新）          | 修改方案 |
| ----------------------------- | ----------------------------- | ------------------- | -------- |
| addOutlineEffect              | setOutline                    | Model / Pawn        |          |
| angleCheck                    | angleCheck                    | MathUtil            |          |
| asyncFindPathToLocation       | findPath                      | Navigation          |          |
| asyncGetCurrentPlayer         | asyncGetLocalPlayer           | Player              |          |
| asyncGetPlayer                | -                             |                     |          |
| boxOverlap                    | boxOverlap                    | QueryUtil           |          |
| boxOverlapInLevel             | boxTrace                      | QueryUtil           |          |
| clearFollow                   | stopFollow                    | Navigation          |          |
| clearMoveTo                   | stopNavigateTo                | Navigation          |          |
| cylinderOverlap               | capsuleOverlap                | QueryUtil           |          |
| follow                        | follow                        | Navigation          |          |
| getAllPlayers                 | getAllPlayers                 | Player              |          |
| getClickGameObjectByScene     | getGameObjectByScreenPosition | ScreenUtil          |          |
| getCurrentPlayer              | localPlayer                   | Player              |          |
| getGameObjectByScenePosition  | getGameObjectByScreenPosition | ScreenUtil          |          |
| getPlayer                     | getPlayer                     | Player              |          |
| getShootDir                   | -                             |                     |          |
| getSightBeadLocation          | getSightBeadPosition          | ScreenUtil          |          |
| isDynamicVibration            | -                             |                     |          |
| lineTrace                     | lineTrace                     | QueryUtil           |          |
| moveTo                        | navigateTo                    | Navigation          |          |
| parabolicTrace                | -                             |                     |          |
| playDynamicForceFeedbackStart | -                             |                     |          |
| playDynamicForceFeedbackStop  | -                             |                     |          |
| removeOutlineEffect           | setOutline                    | Model / Pawn        |          |
| setGlobalAsyncTimeout         | setGlobalAsyncTimeout         | ScriptingSettings   |          |
| setGlobalTimeDilation         | setGlobalTimeDilation         | EnvironmentSettings |          |
| setPlayerPassableForAllArea   | -                             |                     |          |
| setStaticMeshColor            | -                             |                     |          |
| setStaticMeshMaterialColor    | -                             |                     |          |
| spawnNewParticle              | -                             |                     |          |
| spawnNewSound                 | -                             |                     |          |
| sphereOverlap                 | sphereOverlap                 | QueryUtil           |          |

#### [修改]`<类>`GameObjPool 游戏对象池

- 删除getInstance，缩短调用链。
- 删除获取对象池中对象方法`find`

| 接口名称（老） | 属类（老）  | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ----------- | ----------------- | -------------- | ---------- | -------- |
| find           | GameObjPool | MethodDeclaration | -              |            |          |
| getInstance    | GameObjPool | MethodDeclaration | -              |            |          |

#### [删除]`<类>`Group 缓动组

- 删除【Group】对象，替换为【TweenGroup】。

| 接口名称（老） | 属类（老） | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ---------- | ----------------- | -------------- | ---------- | -------- |
| add            | Group      | MethodDeclaration | add            | TweenGroup |          |
| getAll         | Group      | MethodDeclaration | getAll         | TweenGroup |          |
| remove         | Group      | MethodDeclaration | remove         | TweenGroup |          |
| removeAll      | Group      | MethodDeclaration | removeAll      | TweenGroup |          |
| update         | Group      | MethodDeclaration | update         | TweenGroup |          |

#### [修改]`<类>`HitResult 击中结果

- 删除对象击中位置属性`location`，替换为position。

| 接口名称（老） | 属类（老） | 接口类型（老）      | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ---------- | ------------------- | -------------- | ---------- | -------- |
| location       | HitResult  | PropertyDeclaration | position       | HitResult  |          |

#### [修改]`<类>`HotWeapon 热武器

- 合并委托接口，接口名称不再区分C/S端，根据调用端自动绑定。删除老委托接口，替换为新接口。

```
onEquippedClient` & `onEquippedServer` -> `onEquip
onUnequippedClient` & `onUnequippedServer` -> `onUnequip
```

- 删除原先Enable属性，替换为Enabled。
- 删除失效方法`getTransformForFire`。
- 删除设置发射模式方法`setCurrentFireModel`，替换为发射组件`fireComponent`中`fireMode`属性。
- 删除装备/卸载武器方法：`equipment`和`unequipHotWeapon`，替换为`equip`和`unequip`。

| 接口名称（老）       | 属类（老） | 接口类型（老）      | 接口名称（新）         | 属类（新）             | 修改方案 |
| -------------------- | ---------- | ------------------- | ---------------------- | ---------------------- | -------- |
| accuracyOfFireEnable | HotWeapon  | GetAccessor         | accuracyOfFireEnabled  | HotWeapon              |          |
| aimEnable            | HotWeapon  | GetAccessor         | aimEnabled             | HotWeapon              |          |
| loadEnable           | HotWeapon  | GetAccessor         | loadEnabled            | HotWeapon              |          |
| recoilForceEnable    | HotWeapon  | GetAccessor         | recoilForceEnabled     | HotWeapon              |          |
| reloadEnable         | HotWeapon  | GetAccessor         | reloadEnabled          | HotWeapon              |          |
| accuracyOfFireEnable | HotWeapon  | SetAccessor         | accuracyOfFireEnabled  | HotWeapon              |          |
| aimEnable            | HotWeapon  | SetAccessor         | aimEnabled             | HotWeapon              |          |
| loadEnable           | HotWeapon  | SetAccessor         | loadEnabled            | HotWeapon              |          |
| recoilForceEnable    | HotWeapon  | SetAccessor         | recoilForceEnabled     | HotWeapon              |          |
| reloadEnable         | HotWeapon  | SetAccessor         | reloadEnabled          | HotWeapon              |          |
| onEquippedClient     | HotWeapon  | PropertyDeclaration | onEquip                | HotWeapon              |          |
| onEquippedServer     | HotWeapon  | PropertyDeclaration | onEquip                | HotWeapon              |          |
| onUnequippedClient   | HotWeapon  | PropertyDeclaration | onUnequip              | HotWeapon              |          |
| onUnequippedServer   | HotWeapon  | PropertyDeclaration | onUnequip              | HotWeapon              |          |
| equipment            | HotWeapon  | MethodDeclaration   | equip                  | HotWeapon              |          |
| getTransformForFire  | HotWeapon  | MethodDeclaration   | -                      |                        |          |
| setCurrentFireModel  | HotWeapon  | MethodDeclaration   | fireComponent.fireMode | HotWeaponFireComponent |          |
| unequipHotWeapon     | HotWeapon  | MethodDeclaration   | unequip                | HotWeapon              |          |

#### [修改]`<类>`HotWeaponAccuracyOfFireComponent 热武器瞄准组件

- 删除无效方法`bindOpenAccuracyOfFireComponentDelegates`，后续使用委托无需绑定。
- 删除散射值变化委托`onCurrentDispersionChangedClient`，替换为`onCurrentDispersionChange`。

| 接口名称（老）                           | 属类（老）                       | 接口类型（老）      | 接口名称（新）            | 属类（新）                       | 修改方案 |
| ---------------------------------------- | -------------------------------- | ------------------- | ------------------------- | -------------------------------- | -------- |
| bindOpenAccuracyOfFireComponentDelegates | HotWeaponAccuracyOfFireComponent | MethodDeclaration   | -                         |                                  |          |
| onCurrentDispersionChangedClient         | HotWeaponAccuracyOfFireComponent | PropertyDeclaration | onCurrentDispersionChange | HotWeaponAccuracyOfFireComponent |          |

#### [修改]`<类>`HotWeaponAimComponent 热武器瞄准组件

- 删除失效属性瞄准镜UI种类`scopeTypeIndex`。
- 合并委托接口，接口名称不再区分C/S端，根据调用端自动绑定。删除老委托接口，替换为新接口。

```
onAimStartClient` & `onAimStartServer` -> `onStartAim
onAimEndClient` & `onAimEndServer` -> `onEndAim
```

| 接口名称（老）   | 属类（老）            | 接口类型（老）      | 接口名称（新） | 属类（新）            | 修改方案 |
| ---------------- | --------------------- | ------------------- | -------------- | --------------------- | -------- |
| scopeTypeIndex   | HotWeaponAimComponent | GetAccessor         | -              |                       |          |
| scopeTypeIndex   | HotWeaponAimComponent | SetAccessor         | -              |                       |          |
| onAimEndClient   | HotWeaponAimComponent | PropertyDeclaration | onEndAim       | HotWeaponAimComponent |          |
| onAimEndServer   | HotWeaponAimComponent | PropertyDeclaration | onEndAim       | HotWeaponAimComponent |          |
| onAimStartClient | HotWeaponAimComponent | PropertyDeclaration | onStartAim     | HotWeaponAimComponent |          |
| onAimStartServer | HotWeaponAimComponent | PropertyDeclaration | onStartAim     | HotWeaponAimComponent |          |

#### [修改]`<类>`HotWeaponFireComponent 热武器发射组件

- Guid后续统一区分为对象ID：`gameobjectId`；资源ID：`assetId`；删除`animationGuid`，替换为`animationAssetId`。
- 删除原属性current前缀，替换为新属性：`currentBulletSize` -> `currentBullet`；`currentClipSize` -> `clipSize`；`currentFireInterval` -> `fireInterval`；`currentFireModel` -> `fireMode`；`currentMultipleShot` -> `multipleShot`。
- 删除判断全自动模式接口`isFullAutoMode`，替换为发射模式属性`fireMode`完成判断。
- 删除组件内动画ID判断方法`hadAnimationGuid`。
- 合并委托接口，接口名称不再区分C/S端，根据调用端自动绑定。删除老委托接口，替换为新接口。

```
onEndContinuousFireServer` -> `onEndContinuousFire
onEndFireClient` & `onEndFireServer` -> `onEndFire
onStartFireClient` & `onStartFireServer `-> `onStartFire
```

| 接口名称（老）            | 属类（老）             | 接口类型（老）      | 接口名称（新）      | 属类（新）             | 修改方案 |
| ------------------------- | ---------------------- | ------------------- | ------------------- | ---------------------- | -------- |
| animationGuid             | HotWeaponFireComponent | GetAccessor         | animationAssetId    | HotWeaponFireComponent |          |
| currentBulletSize         | HotWeaponFireComponent | GetAccessor         | currentBullet       | HotWeaponFireComponent |          |
| currentClipSize           | HotWeaponFireComponent | GetAccessor         | clipSize            | HotWeaponFireComponent |          |
| currentFireInterval       | HotWeaponFireComponent | GetAccessor         | fireInterval        | HotWeaponFireComponent |          |
| currentFireModel          | HotWeaponFireComponent | GetAccessor         | fireMode            | HotWeaponFireComponent |          |
| currentMultipleShot       | HotWeaponFireComponent | GetAccessor         | multipleShot        | HotWeaponFireComponent |          |
| isFullAutoMode            | HotWeaponFireComponent | GetAccessor         | fireMode            | HotWeaponFireComponent |          |
| animationGuid             | HotWeaponFireComponent | SetAccessor         | animationAssetId    | HotWeaponFireComponent |          |
| currentBulletSize         | HotWeaponFireComponent | SetAccessor         | currentBullet       | HotWeaponFireComponent |          |
| currentClipSize           | HotWeaponFireComponent | SetAccessor         | clipSize            | HotWeaponFireComponent |          |
| currentFireInterval       | HotWeaponFireComponent | SetAccessor         | fireInterval        | HotWeaponFireComponent |          |
| currentMultipleShot       | HotWeaponFireComponent | SetAccessor         | multipleShot        | HotWeaponFireComponent |          |
| isFullAutoMode            | HotWeaponFireComponent | SetAccessor         | fireMode            | HotWeaponFireComponent |          |
| onEndContinuousFireServer | HotWeaponFireComponent | PropertyDeclaration | onEndContinuousFire | HotWeaponFireComponent |          |
| onEndFireClient           | HotWeaponFireComponent | PropertyDeclaration | onEndFire           | HotWeaponFireComponent |          |
| onEndFireServer           | HotWeaponFireComponent | PropertyDeclaration | onEndFire           | HotWeaponFireComponent |          |
| onStartFireClient         | HotWeaponFireComponent | PropertyDeclaration | onStartFire         | HotWeaponFireComponent |          |
| onStartFireServer         | HotWeaponFireComponent | PropertyDeclaration | onStartFire         | HotWeaponFireComponent |          |
| hadAnimationGuid          | HotWeaponFireComponent | MethodDeclaration   | -                   |                        |          |

#### [修改]`<类>`HotWeaponLoadComponent 热武器上膛组件

- Guid后续统一区分为对象ID：`gameobjectId`；资源ID：`assetId`；删除`animationGuid`，替换为`animationAssetId`。
- 删除发射后上膛属性`loadAfterFireEnable`，替换为`loadAfterFireEnabled`。
- 删除组件内动画ID判断方法`hadAnimationGuid`。
- 合并委托接口，接口名称不再区分C/S端，根据调用端自动绑定。删除老委托接口，替换为新接口。

```
onEndLoadClient` & `onEndLoadServer` -> `onEndLoad
onStartLoadClient` & `onStartLoadServer` -> `onStartLoad
```

| 接口名称（老）      | 属类（老）             | 接口类型（老）      | 接口名称（新）       | 属类（新）             | 修改方案 |
| ------------------- | ---------------------- | ------------------- | -------------------- | ---------------------- | -------- |
| animationGuid       | HotWeaponLoadComponent | GetAccessor         | animationAssetId     | HotWeaponLoadComponent |          |
| loadAfterFireEnable | HotWeaponLoadComponent | GetAccessor         | loadAfterFireEnabled | HotWeaponLoadComponent |          |
| animationGuid       | HotWeaponLoadComponent | SetAccessor         | animationAssetId     | HotWeaponLoadComponent |          |
| loadAfterFireEnable | HotWeaponLoadComponent | SetAccessor         | loadAfterFireEnabled | HotWeaponLoadComponent |          |
| onEndLoadClient     | HotWeaponLoadComponent | PropertyDeclaration | onEndLoad            | HotWeaponLoadComponent |          |
| onEndLoadServer     | HotWeaponLoadComponent | PropertyDeclaration | onEndLoad            | HotWeaponLoadComponent |          |
| onStartLoadClient   | HotWeaponLoadComponent | PropertyDeclaration | onStartLoad          | HotWeaponLoadComponent |          |
| onStartLoadServer   | HotWeaponLoadComponent | PropertyDeclaration | onStartLoad          | HotWeaponLoadComponent |          |
| hadAnimationGuid    | HotWeaponLoadComponent | MethodDeclaration   | -                    |                        |          |

#### [修改]`<类>`HotWeaponRecoilForceComponent 热武器后坐力组件

- 合并委托接口，接口名称不再区分C/S端，根据调用端自动绑定。删除老委托接口，替换为新接口。

```
onStartRecoilForceClient` & `onStartRecoilForceServer` -> `onStartRecoil
```

| 接口名称（老）           | 属类（老）                    | 接口类型（老）      | 接口名称（新） | 属类（新）                    | 修改方案 |
| ------------------------ | ----------------------------- | ------------------- | -------------- | ----------------------------- | -------- |
| onStartRecoilForceClient | HotWeaponRecoilForceComponent | PropertyDeclaration | onStartRecoil  | HotWeaponRecoilForceComponent |          |
| onStartRecoilForceServer | HotWeaponRecoilForceComponent | PropertyDeclaration | onStartRecoil  | HotWeaponRecoilForceComponent |          |

#### [修改]`<类>`HotWeaponReloadComponent 热武器换弹组件

- Guid后续统一区分为对象ID：`gameobjectId`；资源ID：`assetId`；删除`animationGuid`，替换为`animationAssetId`。
- 删除组件内动画ID判断方法`hadAnimationGuid`。
- 合并委托接口，接口名称不再区分C/S端，根据调用端自动绑定。删除老委托接口，替换为新接口。

```
onEndReloadClient` & `onEndReloadServer` -> `onEndReload
onStartReloadClient` & `onStartReloadServer` -> `onStartReload
```

| 接口名称（老）      | 属类（老）               | 接口类型（老）      | 接口名称（新）   | 属类（新）               | 修改方案 |
| ------------------- | ------------------------ | ------------------- | ---------------- | ------------------------ | -------- |
| animationGuid       | HotWeaponReloadComponent | GetAccessor         | animationAssetId | HotWeaponReloadComponent |          |
| animationGuid       | HotWeaponReloadComponent | SetAccessor         | animationAssetId | HotWeaponReloadComponent |          |
| onEndReloadClient   | HotWeaponReloadComponent | PropertyDeclaration | onEndReload      | HotWeaponReloadComponent |          |
| onEndReloadServer   | HotWeaponReloadComponent | PropertyDeclaration | onEndReload      | HotWeaponReloadComponent |          |
| onStartReloadClient | HotWeaponReloadComponent | PropertyDeclaration | onStartReload    | HotWeaponReloadComponent |          |
| onStartReloadServer | HotWeaponReloadComponent | PropertyDeclaration | onStartReload    | HotWeaponReloadComponent |          |
| hadAnimationGuid    | HotWeaponReloadComponent | MethodDeclaration   | -                |                          |          |

#### [删除]`<类>`Humanoid 非玩家角色

- 删除【Humanoid】类，统一使用【Character】类代表角色对象，消除角色对象之间的区别。
- 删除`serverCalculateEnable`方法和`serverMovementEnable`属性，替换为【Character】类中新增属性`complexMovementEnabled`。`complexMovementEnabled`默认为true，开启移动组件计算。`complexMovementEnabled`为false，关闭移动组件计算。

| 接口名称（老）        | 属类（老） | 接口类型（老） | 接口名称（新）         | 属类（新） | 修改方案 |
| --------------------- | ---------- | -------------- | ---------------------- | ---------- | -------- |
| serverCalculateEnable | Humanoid   | SetAccessor    | complexMovementEnabled | Character  |          |
| serverMovementEnable  | Humanoid   | SetAccessor    | complexMovementEnabled | Character  |          |

#### [删除]`<类>`HumanoidV1 V1外观 

- 删除【HumanoidV1】等相关的V1角色外观对象。统一使用【CharacterDescription】角色外观描述对象中的`base`对象进行V1角色的外观操作。
- 删除设置姿态方法`changeSomatotype`和`getSomatotype`，后续V1外观只支持设置全身模型。
- 删除V1外观的组成`face`、`hair`、`trunk`，后续V1外观只支持设置全身模型。

| 接口名称（老）   | 属类（老） | 接口类型（老）      | 接口名称（新） | 属类（新）           | 修改方案 |
| ---------------- | ---------- | ------------------- | -------------- | -------------------- | -------- |
| face             | HumanoidV1 | PropertyDeclaration | -              |                      |          |
| hair             | HumanoidV1 | PropertyDeclaration | -              |                      |          |
| trunk            | HumanoidV1 | PropertyDeclaration | -              |                      |          |
| changeSomatotype | HumanoidV1 | MethodDeclaration   | -              |                      |          |
| getSomatotype    | HumanoidV1 | MethodDeclaration   | -              |                      |          |
| getWholeBody     | HumanoidV1 | MethodDeclaration   | base.wholeBody | CharacterDescription |          |
| setWholeBody     | HumanoidV1 | MethodDeclaration   | base.wholeBody | CharacterDescription |          |

#### [删除]`<类>`HumanoidV1Part V1外观基类 

- 删除V1外观的所有类【HumanoidV1Part】【 HumanoidV1Hair】【HumanoidV1Trunk】【HumanoidV1Face】，后续V1外观只支持设置全身模型。

| 接口名称（老） | 属类（老） | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ---------- | ----------------- | -------------- | ---------- | -------- |
| 设置头发模型   | setMesh()  | MethodDeclaration | -              |            |          |
| 获取头发模型   | getMesh()  | MethodDeclaration | -              |            |          |

#### [删除]`<类>`HumanoidV1Hair V1头发 

同HumanoidV1Part V1外观基类

#### [删除]`<类>`HumanoidV1Trunk V1躯干 

同HumanoidV1Part V1外观基类

#### [删除]`<类>`HumanoidV1Face  V1脸 

同HumanoidV1Part V1外观基类

#### [删除]`<类>`HumanoidV2 V2外观 

- 删除【HumanoidV2】等相关的V2角色外观对象。统一使用【CharacterDescription】角色外观描述对象中的`advance`对象进行V2角色的外观操作。

| 接口名称（老）         | 属类（老） | 接口类型（老）      | 接口名称（新）                                       | 属类（新）           | 修改方案 |
| ---------------------- | ---------- | ------------------- | ---------------------------------------------------- | -------------------- | -------- |
| behindHair             | HumanoidV2 | PropertyDeclaration | advance.hair.backHair                                | CharacterDescription |          |
| frontHair              | HumanoidV2 | PropertyDeclaration | advance.hair.frontHair                               | CharacterDescription |          |
| gloves                 | HumanoidV2 | PropertyDeclaration | advance.clothing.gloves                              | CharacterDescription |          |
| head                   | HumanoidV2 | PropertyDeclaration | advance.headFeatures                                 | CharacterDescription |          |
| lowerCloth             | HumanoidV2 | PropertyDeclaration | advance.clothing.lowerCloth                          | CharacterDescription |          |
| shape                  | HumanoidV2 | PropertyDeclaration | advance.bodyFeatures                                 | CharacterDescription |          |
| shoe                   | HumanoidV2 | PropertyDeclaration | advance.clothing.shoes                               | CharacterDescription |          |
| upperCloth             | HumanoidV2 | PropertyDeclaration | advance.clothing.upperCloth                          | CharacterDescription |          |
| appearanceSync         | HumanoidV2 | MethodDeclaration   | syncDescription                                      | Character            |          |
| attach                 | HumanoidV2 | MethodDeclaration   | attachToSlot                                         | Character            |          |
| changeSomatotype       | HumanoidV2 | MethodDeclaration   | advance.base.characterSetting.somatotype             | CharacterDescription |          |
| clearAppearance        | HumanoidV2 | MethodDeclaration   | clearDescription                                     | Character            |          |
| detach                 | HumanoidV2 | MethodDeclaration   | DetachFromSlot                                       | Character            |          |
| getBodyTattooColor     | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalColor           | CharacterDescription |          |
| getBodyTattooPositionX | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalHorizontalShift | CharacterDescription |          |
| getBodyTattooPositionY | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalVerticalShift   | CharacterDescription |          |
| getBodyTattooRotation  | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalOverallRotation | CharacterDescription |          |
| getBodyTattooType      | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalStyle           | CharacterDescription |          |
| getBodyTattooZoom      | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalOverallScale    | CharacterDescription |          |
| getGoods               | HumanoidV2 | MethodDeclaration   | -                                                    |                      |          |
| getSkinColor           | HumanoidV2 | MethodDeclaration   | advance.makeup.skinTone.skinColor                    | CharacterDescription |          |
| getSkinTexture         | HumanoidV2 | MethodDeclaration   | -                                                    |                      |          |
| getSlotWorldPosition   | HumanoidV2 | MethodDeclaration   | getSlotWorldPosition                                 | Character            |          |
| getSomatotype          | HumanoidV2 | MethodDeclaration   | advance.base.characterSetting.somatotype             | CharacterDescription |          |
| getVertexPosition      | HumanoidV2 | MethodDeclaration   | getVertexPosition                                    | Character            |          |
| setAppearanceData      | HumanoidV2 | MethodDeclaration   | setDescription                                       | Character            |          |
| setBodyTattooColor     | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalColor           | CharacterDescription |          |
| setBodyTattooPositionX | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalHorizontalShift | CharacterDescription |          |
| setBodyTattooPositionY | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalVerticalShift   | CharacterDescription |          |
| setBodyTattooRotation  | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalOverallRotation | CharacterDescription |          |
| setBodyTattooType      | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalStyle           | CharacterDescription |          |
| setBodyTattooZoom      | HumanoidV2 | MethodDeclaration   | advance.makeup.bodyDecal[index].decalOverallScale    | CharacterDescription |          |
| setSkinColor           | HumanoidV2 | MethodDeclaration   | advance.makeup.skinTone.skinColor                    | CharacterDescription |          |
| setSkinTexture         | HumanoidV2 | MethodDeclaration   | -                                                    |                      |          |
| setSlot                | HumanoidV2 | MethodDeclaration   | setDescription                                       | Character            |          |
| setSomatotype          | HumanoidV2 | MethodDeclaration   | advance.base.characterSetting.somatotype             | CharacterDescription |          |
| setSuit                | HumanoidV2 | MethodDeclaration   | -                                                    |                      |          |

#### [删除]`<类>`HumanoidV2HairPart V2头发基类 

- 删除继承自【HumanoidV2HairPart】的相关的V2角色发型外观对象。统一使用【CharacterDescription】角色外观描述对象中的`frontHair`和`backHair`对象操作V2角色的发型。下表以`frontHair`为例：

【HumanoidV2FrontHairPart】 -> `advance.hair.frontHair`

【HumanoidV2BehindHairPart 】 -> `advance.hair.backHair`

| 接口名称（老）               | 属类（老）         | 接口类型（老）    | 接口名称（新）                                               | 属类（新）           | 修改方案 |
| ---------------------------- | ------------------ | ----------------- | ------------------------------------------------------------ | -------------------- | -------- |
| getAreaCount                 | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories.length                    | CharacterDescription |          |
| getColor                     | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.color.color                           | CharacterDescription |          |
| getGradientColor             | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.color.gradientColor                   | CharacterDescription |          |
| getGradientIntensity         | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.color.gradientArea                    | CharacterDescription |          |
| getHeaddressColor            | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].color.accessoryColor | CharacterDescription |          |
| getHeaddressDesignColor      | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].design.designColor | CharacterDescription |          |
| getHeaddressDesignRotation   | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].design.designRotation | CharacterDescription |          |
| getHeaddressDesignTexture    | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].design.designStyle | CharacterDescription |          |
| getHeaddressDesignZoom       | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].design.designScale | CharacterDescription |          |
| getHeaddressPatternAngle     | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternRotation | CharacterDescription |          |
| getHeaddressPatternColor     | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternColor | CharacterDescription |          |
| getHeaddressPatternHeight    | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternVerticalScale | CharacterDescription |          |
| getHeaddressPatternIntensity | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternVisibility | CharacterDescription |          |
| getHeaddressPatternTexture   | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternStyle | CharacterDescription |          |
| getHeaddressPatternWidth     | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternHorizontalScale | CharacterDescription |          |
| getHighlightColor            | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.highlight.highlightStyle              | CharacterDescription |          |
| getHighlightMask             | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.highlight.highlightStyle              | CharacterDescription |          |
| getMesh                      | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.style                                 | CharacterDescription |          |
| setColor                     | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.color.color                           | CharacterDescription |          |
| setGradientColor             | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.color.gradientColor                   | CharacterDescription |          |
| setGradientIntensity         | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.color.gradientArea                    | CharacterDescription |          |
| setHeaddressColor            | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].color.accessoryColor | CharacterDescription |          |
| setHeaddressDesignColor      | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].design.designColor | CharacterDescription |          |
| setHeaddressDesignRotation   | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].design.designRotation | CharacterDescription |          |
| setHeaddressDesignTexture    | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].design.designStyle | CharacterDescription |          |
| setHeaddressDesignZoom       | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].design.designScale | CharacterDescription |          |
| setHeaddressPatternAngle     | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternRotation | CharacterDescription |          |
| setHeaddressPatternColor     | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternColor | CharacterDescription |          |
| setHeaddressPatternHeight    | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternVerticalScale | CharacterDescription |          |
| setHeaddressPatternIntensity | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternVisibility | CharacterDescription |          |
| setHeaddressPatternTexture   | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternStyle | CharacterDescription |          |
| setHeaddressPatternWidth     | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.accessories[index].pattern.patternHorizontalScale | CharacterDescription |          |
| setHighlightColor            | HumanoidV2HairPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| setHighlightMask             | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.highlight.highlightStyle              | CharacterDescription |          |
| setMesh                      | HumanoidV2HairPart | MethodDeclaration | advance.hair.frontHair.style                                 | CharacterDescription |          |

#### [删除]`<类>`HumanoidV2FrontHairPart V2前发 

- 见HumanoidV2HairPart 

#### [删除]`<类>`HumanoidV2BehindHairPart V2后发 

- 见HumanoidV2HairPart 

#### [删除]`<类>`HumanoidV2ClothPart V2服装基类 

- 删除继承自【HumanoidV2ClothPart】的相关的V2角色服饰对象。统一使用【CharacterDescription】角色外观描述对象中的`upperCloth`、`lowerCloth`、`gloves`和`shoes`对象操作V2角色的服饰。下表以`upperCloth`为例：

【HumanoidV2UpperClothPart】 -> `advance.clothing.upperCloth`

【HumanoidV2LowerClothPart】 -> `advance.clothing.lowerCloth`

【HumanoidV2GlovesPart】 ->  `advance.clothing.gloves`

【HumanoidV2ShoePart】 -> ` advance.clothing.shoes`

| 接口名称（老）      | 属类（老）          | 接口类型（老）    | 接口名称（新）                                               | 属类（新）           | 修改方案 |
| ------------------- | ------------------- | ----------------- | ------------------------------------------------------------ | -------------------- | -------- |
| getAreaCount        | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part.length                      | CharacterDescription |          |
| getColor            | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].color.areaColor      | CharacterDescription |          |
| getDesignAngle      | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].design.designRotation | CharacterDescription |          |
| getDesignColor      | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].design.designColor   | CharacterDescription |          |
| getDesignTexture    | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].design.designStyle   | CharacterDescription |          |
| getMesh             | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.style                            | CharacterDescription |          |
| getPatternAngle     | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternRotation | CharacterDescription |          |
| getPatternColor     | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternColor | CharacterDescription |          |
| getPatternHeight    | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternVerticalScale | CharacterDescription |          |
| getPatternIntensity | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternVisibility | CharacterDescription |          |
| getPatternWidth     | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternHorizontalScale | CharacterDescription |          |
| getTexture          | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternStyle | CharacterDescription |          |
| setColor            | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].color.areaColor      | CharacterDescription |          |
| setDesignAngle      | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].design.designRotation | CharacterDescription |          |
| setDesignColor      | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].design.designColor   | CharacterDescription |          |
| setDesignTexture    | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].design.designStyle   | CharacterDescription |          |
| setMesh             | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.style                            | CharacterDescription |          |
| setPatternAngle     | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternRotation | CharacterDescription |          |
| setPatternColor     | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternColor | CharacterDescription |          |
| setPatternHeight    | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternVerticalScale | CharacterDescription |          |
| setPatternIntensity | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternVisibility | CharacterDescription |          |
| setPatternWidth     | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternHorizontalScale | CharacterDescription |          |
| setTexture          | HumanoidV2ClothPart | MethodDeclaration | advance.clothing.upperCloth.part[index].pattern.patternStyle | CharacterDescription |          |

#### [删除]`<类>`HumanoidV2UpperClothPart V2上装 

- 见HumanoidV2ClothPart 

#### [删除]`<类>`HumanoidV2LowerClothPart V2 下装 

- 见HumanoidV2ClothPart 

#### [删除]`<类>`HumanoidV2GlovesPart V2手套 

- 见HumanoidV2ClothPart 

#### [删除]`<类>`HumanoidV2ShoePart V2鞋子 

- 见HumanoidV2ClothPart 

#### [删除]`<类>`HumanoidV2HeadPart V2头部 

- 删除继承自【HumanoidV2HeadPart】的相关的V2角色头部外观对象。统一使用【CharacterDescription】角色外观描述对象中的妆容`makeup`和头部特征`headFeatures`对象操作V2角色的头部特征。

| 接口名称（老）              | 属类（老）         | 接口类型（老）    | 接口名称（新）                                               | 属类（新）           | 修改方案 |
| --------------------------- | ------------------ | ----------------- | ------------------------------------------------------------ | -------------------- | -------- |
| characterFaceShadow         | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| getBlushColor               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.blush.blushColor                              | CharacterDescription |          |
| getBlushTexture             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.blush.blushStyle                              | CharacterDescription |          |
| getBrowColor                | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyebrows.eyebrowColor                         | CharacterDescription |          |
| getBrowTexture              | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyebrows.eyebrowStyle                         | CharacterDescription |          |
| getExpression               | HumanoidV2HeadPart | MethodDeclaration | advance.headFeatures.expressions.changeExpression            | CharacterDescription |          |
| getEyeHighlightColor        | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| getEyeHighlightTexture      | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| getEyeShadowColor           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyeShadow.eyeshaowColor                       | CharacterDescription |          |
| getEyeShadowTexture         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyeShadow.eyeshadowStyle                      | CharacterDescription |          |
| getEyeTexture               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.pupilStyle              | CharacterDescription |          |
| getEyelashColor             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyelashes.eyelashColor                        | CharacterDescription |          |
| getEyelashTexture           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyelashes.eyelashStyle                        | CharacterDescription |          |
| getFacialTattooColor        | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalColor                   | CharacterDescription |          |
| getFacialTattooPositionX    | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalVerticalShift           | CharacterDescription |          |
| getFacialTattooPositionY    | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalOverallRotation         | CharacterDescription |          |
| getFacialTattooRotation     | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalStyle                   | CharacterDescription |          |
| getFacialTattooType         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalOverallScale            | CharacterDescription |          |
| getFacialTattooZoom         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.headDecal.decalColor                          | CharacterDescription |          |
| getHeadPatternColor         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.headDecal.decalStyle                          | CharacterDescription |          |
| getHeadPatternTexture       | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.leftPupilColor          | CharacterDescription |          |
| getLeftEyeColor             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.lipstick.lipstickColor                        | CharacterDescription |          |
| getLipstickColor            | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.lipstick.lipstickStyle                        | CharacterDescription |          |
| getLipstickTexture          | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.lowerHighlightColor | CharacterDescription |          |
| getLowerEyeHighlightColor   | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.lowerHighlightStyle | CharacterDescription |          |
| getLowerEyeHighlightTexture | HumanoidV2HeadPart | MethodDeclaration | advance.headFeatures.head.style                              | CharacterDescription |          |
| getMesh                     | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilColor              | CharacterDescription |          |
| getPupilColor               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilHorizontalPosition | CharacterDescription |          |
| getPupilPositionX           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilVerticalPosition   | CharacterDescription |          |
| getPupilPositionY           | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| getPupilRotate              | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilSizeScale          | CharacterDescription |          |
| getPupilScale               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilStyle              | CharacterDescription |          |
| getPupilTexture             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.rightPupilColor         | CharacterDescription |          |
| getRightEyeColor            | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.upperHighlightColor | CharacterDescription |          |
| getUpperEyeHighlightColor   | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.upperHighlightStyle | CharacterDescription |          |
| getUpperEyeHighlightTexture | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.blush.blushColor                              | CharacterDescription |          |
| setBlushColor               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.blush.blushStyle                              | CharacterDescription |          |
| setBlushTexture             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyebrows.eyebrowColor                         | CharacterDescription |          |
| setBrowColor                | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyebrows.eyebrowStyle                         | CharacterDescription |          |
| setBrowTexture              | HumanoidV2HeadPart | MethodDeclaration | advance.headFeatures.expressions.changeExpression            | CharacterDescription |          |
| setEyeHighlightColor        | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| setEyeHighlightTexture      | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| setEyeShadowColor           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyeShadow.eyeshaowColor                       | CharacterDescription |          |
| setEyeShadowTexture         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyeShadow.eyeshadowStyle                      | CharacterDescription |          |
| setEyeTexture               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.pupilStyle              | CharacterDescription |          |
| setEyelashColor             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyelashes.eyelashColor                        | CharacterDescription |          |
| setEyelashTexture           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyelashes.eyelashStyle                        | CharacterDescription |          |
| setFacialTattooColor        | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalColor                   | CharacterDescription |          |
| setFacialTattooPositionX    | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalHorizontalShift         | CharacterDescription |          |
| setFacialTattooPositionY    | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalVerticalShift           | CharacterDescription |          |
| setFacialTattooRotation     | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalOverallRotation         | CharacterDescription |          |
| setFacialTattooType         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalStyle                   | CharacterDescription |          |
| setFacialTattooZoom         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalOverallScale            | CharacterDescription |          |
| setHeadPatternColor         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.headDecal.decalColor                          | CharacterDescription |          |
| setHeadPatternTexture       | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.headDecal.decalStyle                          | CharacterDescription |          |
| setLeftEyeColor             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.leftPupilColor          | CharacterDescription |          |
| setLipstickColor            | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.lipstick.lipstickColor                        | CharacterDescription |          |
| setLipstickTexture          | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.lipstick.lipstickStyle                        | CharacterDescription |          |
| setLowerEyeHighlightColor   | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.lowerHighlightColor | CharacterDescription |          |
| setLowerEyeHighlightTexture | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.lowerHighlightStyle | CharacterDescription |          |
| setMesh                     | HumanoidV2HeadPart | MethodDeclaration | advance.headFeatures.head.style                              | CharacterDescription |          |
| setPupilColor               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilColor              | CharacterDescription |          |
| setPupilPositionX           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilHorizontalPosition | CharacterDescription |          |
| setPupilPositionY           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilVerticalPosition   | CharacterDescription |          |
| setPupilRotate              | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| setPupilScale               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilSizeScale          | CharacterDescription |          |
| setPupilTexture             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilStyle              | CharacterDescription |          |
| setRightEyeColor            | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.rightPupilColor         | CharacterDescription |          |
| setUpperEyeHighlightColor   | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.upperHighlightColor | CharacterDescription |          |
| setUpperEyeHighlightTexture | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.upperHighlightStyle | CharacterDescription |          |

#### [删除]`<类>`HumanoidV2UpperClothPart V2上装 

- 见HumanoidV2ClothPart 

#### [删除]`<类>`HumanoidV2LowerClothPart V2 下装 

- 见HumanoidV2ClothPart 

#### [删除]`<类>`HumanoidV2GlovesPart V2手套 

- 见HumanoidV2ClothPart 

#### [删除]`<类>`HumanoidV2ShoePart V2鞋子 

- 见HumanoidV2ClothPart 

#### [删除]`<类>`HumanoidV2HeadPart V2头部 

- 删除继承自【HumanoidV2HeadPart】的相关的V2角色头部外观对象。统一使用【CharacterDescription】角色外观描述对象中的妆容`makeup`和头部特征`headFeatures`对象操作V2角色的头部特征。

| 接口名称（老）              | 属类（老）         | 接口类型（老）    | 接口名称（新）                                               | 属类（新）           | 修改方案 |
| --------------------------- | ------------------ | ----------------- | ------------------------------------------------------------ | -------------------- | -------- |
| characterFaceShadow         | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| getBlushColor               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.blush.blushColor                              | CharacterDescription |          |
| getBlushTexture             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.blush.blushStyle                              | CharacterDescription |          |
| getBrowColor                | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyebrows.eyebrowColor                         | CharacterDescription |          |
| getBrowTexture              | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyebrows.eyebrowStyle                         | CharacterDescription |          |
| getExpression               | HumanoidV2HeadPart | MethodDeclaration | advance.headFeatures.expressions.changeExpression            | CharacterDescription |          |
| getEyeHighlightColor        | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| getEyeHighlightTexture      | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| getEyeShadowColor           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyeShadow.eyeshaowColor                       | CharacterDescription |          |
| getEyeShadowTexture         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyeShadow.eyeshadowStyle                      | CharacterDescription |          |
| getEyeTexture               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.pupilStyle              | CharacterDescription |          |
| getEyelashColor             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyelashes.eyelashColor                        | CharacterDescription |          |
| getEyelashTexture           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyelashes.eyelashStyle                        | CharacterDescription |          |
| getFacialTattooColor        | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalColor                   | CharacterDescription |          |
| getFacialTattooPositionX    | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalVerticalShift           | CharacterDescription |          |
| getFacialTattooPositionY    | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalOverallRotation         | CharacterDescription |          |
| getFacialTattooRotation     | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalStyle                   | CharacterDescription |          |
| getFacialTattooType         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalOverallScale            | CharacterDescription |          |
| getFacialTattooZoom         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.headDecal.decalColor                          | CharacterDescription |          |
| getHeadPatternColor         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.headDecal.decalStyle                          | CharacterDescription |          |
| getHeadPatternTexture       | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.leftPupilColor          | CharacterDescription |          |
| getLeftEyeColor             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.lipstick.lipstickColor                        | CharacterDescription |          |
| getLipstickColor            | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.lipstick.lipstickStyle                        | CharacterDescription |          |
| getLipstickTexture          | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.lowerHighlightColor | CharacterDescription |          |
| getLowerEyeHighlightColor   | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.lowerHighlightStyle | CharacterDescription |          |
| getLowerEyeHighlightTexture | HumanoidV2HeadPart | MethodDeclaration | advance.headFeatures.head.style                              | CharacterDescription |          |
| getMesh                     | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilColor              | CharacterDescription |          |
| getPupilColor               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilHorizontalPosition | CharacterDescription |          |
| getPupilPositionX           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilVerticalPosition   | CharacterDescription |          |
| getPupilPositionY           | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| getPupilRotate              | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilSizeScale          | CharacterDescription |          |
| getPupilScale               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilStyle              | CharacterDescription |          |
| getPupilTexture             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.rightPupilColor         | CharacterDescription |          |
| getRightEyeColor            | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.upperHighlightColor | CharacterDescription |          |
| getUpperEyeHighlightColor   | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.upperHighlightStyle | CharacterDescription |          |
| getUpperEyeHighlightTexture | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.blush.blushColor                              | CharacterDescription |          |
| setBlushColor               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.blush.blushStyle                              | CharacterDescription |          |
| setBlushTexture             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyebrows.eyebrowColor                         | CharacterDescription |          |
| setBrowColor                | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyebrows.eyebrowStyle                         | CharacterDescription |          |
| setBrowTexture              | HumanoidV2HeadPart | MethodDeclaration | advance.headFeatures.expressions.changeExpression            | CharacterDescription |          |
| setEyeHighlightColor        | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| setEyeHighlightTexture      | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| setEyeShadowColor           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyeShadow.eyeshaowColor                       | CharacterDescription |          |
| setEyeShadowTexture         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyeShadow.eyeshadowStyle                      | CharacterDescription |          |
| setEyeTexture               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.pupilStyle              | CharacterDescription |          |
| setEyelashColor             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyelashes.eyelashColor                        | CharacterDescription |          |
| setEyelashTexture           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.eyelashes.eyelashStyle                        | CharacterDescription |          |
| setFacialTattooColor        | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalColor                   | CharacterDescription |          |
| setFacialTattooPositionX    | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalHorizontalShift         | CharacterDescription |          |
| setFacialTattooPositionY    | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalVerticalShift           | CharacterDescription |          |
| setFacialTattooRotation     | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalOverallRotation         | CharacterDescription |          |
| setFacialTattooType         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalStyle                   | CharacterDescription |          |
| setFacialTattooZoom         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.faceDecal[index].decalOverallScale            | CharacterDescription |          |
| setHeadPatternColor         | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.headDecal.decalColor                          | CharacterDescription |          |
| setHeadPatternTexture       | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.headDecal.decalStyle                          | CharacterDescription |          |
| setLeftEyeColor             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.leftPupilColor          | CharacterDescription |          |
| setLipstickColor            | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.lipstick.lipstickColor                        | CharacterDescription |          |
| setLipstickTexture          | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.lipstick.lipstickStyle                        | CharacterDescription |          |
| setLowerEyeHighlightColor   | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.lowerHighlightColor | CharacterDescription |          |
| setLowerEyeHighlightTexture | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.lowerHighlightStyle | CharacterDescription |          |
| setMesh                     | HumanoidV2HeadPart | MethodDeclaration | advance.headFeatures.head.style                              | CharacterDescription |          |
| setPupilColor               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilColor              | CharacterDescription |          |
| setPupilPositionX           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilHorizontalPosition | CharacterDescription |          |
| setPupilPositionY           | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilVerticalPosition   | CharacterDescription |          |
| setPupilRotate              | HumanoidV2HeadPart | MethodDeclaration | -                                                            | CharacterDescription |          |
| setPupilScale               | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilSizeScale          | CharacterDescription |          |
| setPupilTexture             | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.decal.pupilStyle              | CharacterDescription |          |
| setRightEyeColor            | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.style.rightPupilColor         | CharacterDescription |          |
| setUpperEyeHighlightColor   | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.upperHighlightColor | CharacterDescription |          |
| setUpperEyeHighlightTexture | HumanoidV2HeadPart | MethodDeclaration | advance.makeup.coloredContacts.highlight.upperHighlightStyle | CharacterDescription |          |

#### [删除]`<类>`HumanoidV2Shape V2 体型

- 删除继承自【HumanoidV2Shape】的相关的V2角色身体外观对象。统一使用【CharacterDescription】角色外观描述对象中的`bodyFeatures`对象操作V2角色的身体特征。

| 接口名称（老）               | 属类（老）      | 接口类型（老）    | 接口名称（新）                                               | 属类（新）           | 修改方案 |
| ---------------------------- | --------------- | ----------------- | ------------------------------------------------------------ | -------------------- | -------- |
| getBreastHorizontalPosition  | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.breast.breastHorizontalShift            | CharacterDescription |          |
| getBreastLength              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.breast.breastLength                     | CharacterDescription |          |
| getBreastScale               | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.breast.breastOverallScale               | CharacterDescription |          |
| getBreastStretch             | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.chest.chestVerticalScale                | CharacterDescription |          |
| getBreastVerticalPosition    | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.breast.breastVerticalShift              | CharacterDescription |          |
| getBrowGap                   | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowHorizontalShift         | CharacterDescription |          |
| getBrowHeight                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowVerticalShift           | CharacterDescription |          |
| getBrowInboardShape          | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowOuterVerticalShift      | CharacterDescription |          |
| getBrowOutsideShape          | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowOverallRotation         | CharacterDescription |          |
| getBrowRotation              | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.eyeCorners.outerEyeCornerVerticalShift | CharacterDescription |          |
| getCanthusHorizontalPosition | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.eyeCorners.innerEyeCornerHorizontalShift | CharacterDescription |          |
| getCanthusVerticalPosition   | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.body.height                             | CharacterDescription |          |
| getCharacterHeight           | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheekbone.cheekboneFrontalShift | CharacterDescription |          |
| getCheekBoneRange            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheekbone.cheekboneHorizontalScale | CharacterDescription |          |
| getCheekBoneWidth            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheek.cheekVerticalShift      | CharacterDescription |          |
| getCheekHeight               | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheek.cheekFrontalShift       | CharacterDescription |          |
| getCheekRange                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheek.cheekHorizontalScale    | CharacterDescription |          |
| getCheekWidth                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earHorizontalRotation              | CharacterDescription |          |
| getEarRoll                   | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earOverallScale                    | CharacterDescription |          |
| getEarScale                  | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earFrontalRotation                 | CharacterDescription |          |
| getEarYaw                    | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeHorizontalShift         | CharacterDescription |          |
| getEyesGap                   | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeVerticalShift           | CharacterDescription |          |
| getEyesHeight                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeVerticalScale           | CharacterDescription |          |
| getEyesLength                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeFrontalShift            | CharacterDescription |          |
| getEyesRange                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeOverallRotation         | CharacterDescription |          |
| getEyesRotation              | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeHorizontalScale         | CharacterDescription |          |
| getEyesWidth                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.overall.faceHorizontalScale   | CharacterDescription |          |
| getFaceWidth                 | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.feet.feetOverallScale                   | CharacterDescription |          |
| getFootScale                 | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.hips.hipFrontalScale                    | CharacterDescription |          |
| getGroinThickness            | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.hips.hipHorizontalScale                 | CharacterDescription |          |
| getGroinWidth                | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.hands.handOverallScale                  | CharacterDescription |          |
| getHandScale                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.head.headOverallScale                   | CharacterDescription |          |
| getHeadScale                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.jawline.jawlineVerticalShift  | CharacterDescription |          |
| getJawLength                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.chin.chinFrontalShift         | CharacterDescription |          |
| getJawRange                  | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.jawline.jawlineRoundness      | CharacterDescription |          |
| getJawSmooth                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.chin.chinTipVerticalShift     | CharacterDescription |          |
| getJawVertexHeight           | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.chin.chinTipFrontalShift      | CharacterDescription |          |
| getJawVertexRange            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.chin.chinTipHorizontalScale   | CharacterDescription |          |
| getJawVertexWidth            | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.forearmVerticalScale               | CharacterDescription |          |
| getLowerArmsStretch          | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.forearmFrontalScale                | CharacterDescription |          |
| getLowerArmsThickness        | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.forearmHorizontalScale             | CharacterDescription |          |
| getLowerArmsWidth            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.overall.lowerFaceFrontalShift | CharacterDescription |          |
| getLowerFaceRange            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.overall.lowerFaceHorizontalScale | CharacterDescription |          |
| getLowerFaceWidth            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.jawline.jawFrontalShift       | CharacterDescription |          |
| getLowerJawRange             | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.jawline.jawHorizontalScale    | CharacterDescription |          |
| getLowerJawWidth             | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.lips.lowerLipThickness            | CharacterDescription |          |
| getLowerMouthThickness       | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earLowerScale                      | CharacterDescription |          |
| getLowerStretch              | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.overall.mouthVerticalShift        | CharacterDescription |          |
| getMouthHeight               | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.overall.mouthFrontalShift         | CharacterDescription |          |
| getMouthRange                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.mouthCorners.mouthCornerVerticalShiftadvance.headFeatures.mouth.mouthCorners.mouthCornerFrontalShift | CharacterDescription |          |
| getMouthShape                | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.neck.neckVerticalScale                  | CharacterDescription |          |
| getMouthWidth                | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.neck.neckFrontalScale                   | CharacterDescription |          |
| getNeckStretch               | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.neck.neckHorizontalScale                | CharacterDescription |          |
| getNeckThickness             | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.nose.noseBridge.noseBridgeFrontalShift  | CharacterDescription |          |
| getNeckWidth                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.nose.noseTip.noseTipVerticalShift       | CharacterDescription |          |
| getNoseHeight                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.nose.overall.noseOverallVerticalShift   | CharacterDescription |          |
| getNoseProtrusion            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.pupils.pupilVerticalScale          | CharacterDescription |          |
| getNoseVerticalPosition      | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.pupils.pupilHorizontalShift        | CharacterDescription |          |
| getPupilHeight               | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.pupils.pupilVerticalShift          | CharacterDescription |          |
| getPupilHorizontalPosition   | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.pupils.pupilHorizontalScale        | CharacterDescription |          |
| getPupilVerticalPosition     | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.ribs.ribFrontalScale                    | CharacterDescription |          |
| getPupilWidth                | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.ribs.ribHorizontalScale                 | CharacterDescription |          |
| getRibThickness              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.calfHorizontalScale                | CharacterDescription |          |
| getRibWidth                  | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.calfFrontalScale                   | CharacterDescription |          |
| getShankScaleX               | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.calfVerticalScale                  | CharacterDescription |          |
| getShankScaleZ               | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.shoulderFrontalScale               | CharacterDescription |          |
| getShankStretch              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.shoulderHorizontalScale            | CharacterDescription |          |
| getShoulderArmThickness      | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.chest.chestFrontalScale                 | CharacterDescription |          |
| getShoulderArmWidth          | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.chest.chestHorizontalScale              | CharacterDescription |          |
| getShoulderThickness         | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.thighVerticalScale                 | CharacterDescription |          |
| getShoulderWidth             | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.thighHorizontalScale               | CharacterDescription |          |
| getThighStretch              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.thighFrontalScale                  | CharacterDescription |          |
| getThighThicknessX           | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.upperArmVerticalScale              | CharacterDescription |          |
| getThighThicknessZ           | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.upperArmFrontalScale               | CharacterDescription |          |
| getUpperArmsStretch          | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.upperArmHorizontalScale            | CharacterDescription |          |
| getUpperArmsThickness        | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.overall.upperFaceFrontalShift | CharacterDescription |          |
| getUpperArmsWidth            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.lips.upperLipThickness            | CharacterDescription |          |
| getUpperFaceRange            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earUpperScale                      | CharacterDescription |          |
| getUpperMouthThickness       | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.waist.waistVerticalScale                | CharacterDescription |          |
| getUpperStretch              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.waist.waistFrontalScale                 | CharacterDescription |          |
| getWaistStretch              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.waist.waistHorizontalScale              | CharacterDescription |          |
| getWaistThickness            | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.breast.breastHorizontalShift            | CharacterDescription |          |
| getWaistWidth                | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.breast.breastLength                     | CharacterDescription |          |
| setBreastHorizontalPosition  | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.breast.breastOverallScale               | CharacterDescription |          |
| setBreastLength              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.chest.chestVerticalScale                | CharacterDescription |          |
| setBreastScale               | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.breast.breastVerticalShift              | CharacterDescription |          |
| setBreastStretch             | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowHorizontalShift         | CharacterDescription |          |
| setBreastVerticalPosition    | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowVerticalShift           | CharacterDescription |          |
| setBrowGap                   | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowInnerVerticalShift      | CharacterDescription |          |
| setBrowHeight                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowInnerVerticalShift      | CharacterDescription |          |
| setBrowInboardShape          | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowOuterVerticalShift      | CharacterDescription |          |
| setBrowOutsideShape          | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyebrows.eyebrowOverallRotation         | CharacterDescription |          |
| setBrowRotation              | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeHorizontalScale         | CharacterDescription |          |
| setCanthusHorizontalPosition | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.eyeCorners.outerEyeCornerVerticalShift | CharacterDescription |          |
| setCanthusVerticalPosition   | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.body.height                             | CharacterDescription |          |
| setCharacterHeight           | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheekbone.cheekboneFrontalShift | CharacterDescription |          |
| setCheekBoneRange            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheekbone.cheekboneHorizontalScale | CharacterDescription |          |
| setCheekBoneWidth            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheek.cheekVerticalShift      | CharacterDescription |          |
| setCheekHeight               | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheek.cheekFrontalShift       | CharacterDescription |          |
| setCheekRange                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.cheek.cheekHorizontalScale    | CharacterDescription |          |
| setCheekWidth                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earHorizontalRotation              | CharacterDescription |          |
| setEarRoll                   | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earOverallScale                    | CharacterDescription |          |
| setEarScale                  | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earFrontalRotation                 | CharacterDescription |          |
| setEarYaw                    | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeHorizontalShift         | CharacterDescription |          |
| setEyesGap                   | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeVerticalShift           | CharacterDescription |          |
| setEyesHeight                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeVerticalScale           | CharacterDescription |          |
| setEyesLength                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeFrontalShift            | CharacterDescription |          |
| setEyesRange                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeOverallRotation         | CharacterDescription |          |
| setEyesRotation              | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.overall.eyeHorizontalScale         | CharacterDescription |          |
| setEyesWidth                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.overall.faceHorizontalScale   | CharacterDescription |          |
| setFaceWidth                 | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.feet.feetOverallScale                   | CharacterDescription |          |
| setFootScale                 | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.hips.hipFrontalScale                    | CharacterDescription |          |
| setGroinThickness            | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.hips.hipHorizontalScale                 | CharacterDescription |          |
| setGroinWidth                | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.hands.handOverallScale                  | CharacterDescription |          |
| setHandScale                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.head.headOverallScale                   | CharacterDescription |          |
| setHeadScale                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.jawline.jawlineVerticalShift  | CharacterDescription |          |
| setJawLength                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.chin.chinFrontalShift         | CharacterDescription |          |
| setJawRange                  | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.jawline.jawlineRoundness      | CharacterDescription |          |
| setJawSmooth                 | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.chin.chinTipVerticalShift     | CharacterDescription |          |
| setJawVertexHeight           | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.chin.chinTipFrontalShift      | CharacterDescription |          |
| setJawVertexRange            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.chin.chinTipHorizontalScale   | CharacterDescription |          |
| setJawVertexWidth            | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.forearmVerticalScale               | CharacterDescription |          |
| setLowerArmsStretch          | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.forearmFrontalScale                | CharacterDescription |          |
| setLowerArmsThickness        | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.forearmHorizontalScale             | CharacterDescription |          |
| setLowerArmsWidth            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.overall.lowerFaceFrontalShift | CharacterDescription |          |
| setLowerFaceRange            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.overall.lowerFaceHorizontalScale | CharacterDescription |          |
| setLowerFaceWidth            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.jawline.jawFrontalShift       | CharacterDescription |          |
| setLowerJawRange             | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.jawline.jawHorizontalScale    | CharacterDescription |          |
| setLowerJawWidth             | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.lips.lowerLipThickness            | CharacterDescription |          |
| setLowerMouthThickness       | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earLowerScale                      | CharacterDescription |          |
| setLowerStretch              | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.overall.mouthVerticalShift        | CharacterDescription |          |
| setMouthHeight               | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.overall.mouthFrontalShift         | CharacterDescription |          |
| setMouthRange                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.mouthCorners.mouthCornerVerticalShift | CharacterDescription |          |
| setMouthShape                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.overall.mouthHorizontalScale      | CharacterDescription |          |
| setMouthWidth                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.overall.mouthHorizontalScale      | CharacterDescription |          |
| setNeckStretch               | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.neck.neckVerticalScalesetBreastStretch  | CharacterDescription |          |
| setNeckThickness             | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.neck.neckFrontalScale                   | CharacterDescription |          |
| setNeckWidth                 | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.neck.neckHorizontalScale                | CharacterDescription |          |
| setNoseHeight                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.nose.noseBridge.noseBridgeFrontalShift  | CharacterDescription |          |
| setNoseProtrusion            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.nose.noseTip.noseTipVerticalShift       | CharacterDescription |          |
| setNoseVerticalPosition      | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.nose.overall.noseOverallVerticalShift   | CharacterDescription |          |
| setPupilHeight               | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.pupils.pupilVerticalScale          | CharacterDescription |          |
| setPupilHorizontalPosition   | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.pupils.pupilHorizontalShift        | CharacterDescription |          |
| setPupilVerticalPosition     | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.pupils.pupilVerticalShift          | CharacterDescription |          |
| setPupilWidth                | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.eyes.pupils.pupilHorizontalScale        | CharacterDescription |          |
| setRibThickness              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.ribs.ribFrontalScale                    | CharacterDescription |          |
| setRibWidth                  | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.ribs.ribHorizontalScale                 | CharacterDescription |          |
| setShankScaleX               | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.calfHorizontalScale                | CharacterDescription |          |
| setShankScaleZ               | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.calfFrontalScale                   | CharacterDescription |          |
| setShankStretch              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.calfVerticalScale                  | CharacterDescription |          |
| setShoulderArmThickness      | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.shoulderFrontalScale               | CharacterDescription |          |
| setShoulderArmWidth          | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.shoulderHorizontalScale            | CharacterDescription |          |
| setShoulderThickness         | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.chest.chestFrontalScale                 | CharacterDescription |          |
| setShoulderWidth             | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.chest.chestHorizontalScale              | CharacterDescription |          |
| setThighStretch              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.thighVerticalScale                 | CharacterDescription |          |
| setThighThicknessX           | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.thighHorizontalScale               | CharacterDescription |          |
| setThighThicknessZ           | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.legs.thighFrontalScale                  | CharacterDescription |          |
| setUpperArmsStretch          | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.upperArmVerticalScale              | CharacterDescription |          |
| setUpperArmsThickness        | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.upperArmFrontalScale               | CharacterDescription |          |
| setUpperArmsWidth            | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.arms.upperArmHorizontalScale            | CharacterDescription |          |
| setUpperFaceRange            | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.faceShape.overall.upperFaceFrontalShift | CharacterDescription |          |
| setUpperMouthThickness       | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.mouth.lips.upperLipThickness            | CharacterDescription |          |
| setUpperStretch              | HumanoidV2Shape | MethodDeclaration | advance.headFeatures.ears.earUpperScale                      | CharacterDescription |          |
| setWaistStretch              | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.waist.waistVerticalScale                | CharacterDescription |          |
| setWaistThickness            | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.waist.waistFrontalScale                 | CharacterDescription |          |
| setWaistWidth                | HumanoidV2Shape | MethodDeclaration | advance.bodyFeatures.waist.waistHorizontalScale              | CharacterDescription |          |

#### [修改]`<类>`Interactor 交互物

- 删除交互姿态`interactiveSlot`，替换为为交互动画animationId，支持动画资源输入（兼容姿态输入）。
- 优化交互物交互的延迟时间（仅动画资源输入可以生效，姿态资源还是有延迟）
- 规范交互物的RPC能力。对于双端交互物：服务端调用广播，客户端调用如果是玩家主控端则广播，否则不生效。对于单端交互物：暂不保证交互效果。

| 接口名称（老）        | 属类（老） | 接口类型（老）      | 接口名称（新）      | 属类（新） | 修改方案 |
| --------------------- | ---------- | ------------------- | ------------------- | ---------- | -------- |
| interactiveSlot       | Interactor | GetAccessor         | slot                | Interactor |          |
| interactiveStance     | Interactor | GetAccessor         | animationId         | Interactor |          |
| interactiveSlot       | Interactor | SetAccessor         | slot                | Interactor |          |
| interactiveStance     | Interactor | SetAccessor         | animationId         | Interactor |          |
| onInteractiveEnded    | Interactor | PropertyDeclaration | onLeave             | Interactor |          |
| onInteractiveStarted  | Interactor | PropertyDeclaration | onEnter             | Interactor |          |
| onInteractorEnter     | Interactor | PropertyDeclaration | onEnter             | Interactor |          |
| onInteractorExit      | Interactor | PropertyDeclaration | onLeave             | Interactor |          |
| endInteract           | Interactor | MethodDeclaration   | leave               | Interactor |          |
| enterInteractiveState | Interactor | MethodDeclaration   | enter               | Interactor |          |
| exitInteractiveState  | Interactor | MethodDeclaration   | leave               | Interactor |          |
| getInteractCharacter  | Interactor | MethodDeclaration   | getCurrentCharacter | Interactor |          |
| getInteractiveState   | Interactor | MethodDeclaration   | occupied            | Interactor |          |
| getInteractiveStatus  | Interactor | MethodDeclaration   | occupied            | Interactor |          |
| interactiveCharacter  | Interactor | MethodDeclaration   | getCurrentCharacter | Interactor |          |
| startInteract         | Interactor | MethodDeclaration   | enter               | Interactor |          |

#### [修改]`<类>`MessageChannelService 消息频道服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。

| 接口名称（老） | 属类（老）            | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | --------------------- | ----------------- | -------------- | ---------- | -------- |
| getInstance    | MessageChannelService | MethodDeclaration | -              |            |          |

#### [修改]`<类>`Mesh 模型

- 删除【Mesh】模型对象，接口迁移至新增对象【Model】。
- 删除无效属性`applyImpulseOnDamage`、`ignoreRadialForce`和`ignoreRadialImpulse`。
- 启用某功能的属性统一替换为Enabled后缀，此外属性名称简化。

```
gravityEnable` -> `gravityEnabled
isSimulatingPhysics` -> `physicsEnabled
massEnable` -> `massEnabled
massInKg` -> `mass
```

- 删除检测进入和离开模型事件委托`onEnter`和`onLeave`，替换为`onTouch`和`onTouchEnd`，方便后续接入碰撞查询结果（目前当且仅当“仅检测”才能触发事件）。
- 删除设置模型颜色方法`setMaterialColor`和`getMaterialColor`。后续需通过【MaterialInstance】中的接口中的`setVectorParameterValue`和`getVectorParameterValue`替换。
- 新增`setOutline`方法和`setPostProcessOutline`方法，给模型绘制描边。

| 接口名称（老）         | 属类（老） | 接口类型（老）      | 接口名称（新）         | 属类（新） | 修改方案 |
| ---------------------- | ---------- | ------------------- | ---------------------- | ---------- | -------- |
| applyImpulseOnDamage   | Mesh       | GetAccessor         | -                      |            |          |
| gravityEnable          | Mesh       | GetAccessor         | gravityEnabled         | Model      |          |
| ignoreRadialForce      | Mesh       | GetAccessor         | -                      |            |          |
| ignoreRadialImpulse    | Mesh       | GetAccessor         | -                      |            |          |
| isSimulatingPhysics    | Mesh       | GetAccessor         | physicsEnabled         | Model      |          |
| massEnable             | Mesh       | GetAccessor         | massEnabled            | Model      |          |
| massInKg               | Mesh       | GetAccessor         | mass                   | Model      |          |
| applyImpulseOnDamage   | Mesh       | SetAccessor         | -                      |            |          |
| gravityEnable          | Mesh       | SetAccessor         | gravityEnabled         | Model      |          |
| ignoreRadialForce      | Mesh       | SetAccessor         | -                      |            |          |
| ignoreRadialImpulse    | Mesh       | SetAccessor         | -                      |            |          |
| isSimulatingPhysics    | Mesh       | SetAccessor         | physicsEnabled         | Model      |          |
| massEnable             | Mesh       | SetAccessor         | massEnabled            | Model      |          |
| massInKg               | Mesh       | SetAccessor         | mass                   | Model      |          |
| onEnter                | Mesh       | PropertyDeclaration | onTouch                | Model      |          |
| onLeave                | Mesh       | PropertyDeclaration | onTouchEnd             | Model      |          |
| createMaterialInstance | Mesh       | MethodDeclaration   | createMaterialInstance | Model      |          |
| getMaterialColor       | Mesh       | MethodDeclaration   | -                      |            |          |
| getMaterialInstance    | Mesh       | MethodDeclaration   | getMaterialInstance    | Model      |          |
| resetMaterial          | Mesh       | MethodDeclaration   | resetMaterial          | Model      |          |
| setCullDistance        | Mesh       | MethodDeclaration   | setCullDistance        | Model      |          |
| setMaterialColor       | Mesh       | MethodDeclaration   | -                      |            |          |
| setOutlineAndColor     | Mesh       | MethodDeclaration   | setOutline             | Model      |          |
| setStaticMeshAsset     | Mesh       | MethodDeclaration   | setStaticMeshAsset     | Model      |          |

#### [删除]`<类>`MobileSensor 移动传感器

- 删除低频使用对象【MobileSensor】移动传感器

#### [删除]`<类>`NPC 非玩家角色

- 删除【NPC】类，统一使用【Character】类代表角色对象，消除角色对象之间的区别。
- 删除NPC专属接口。

| 接口名称（老）          | 属类（老） | 接口类型（老）    | 接口名称（新）         | 属类（新） | 修改方案 |
| ----------------------- | ---------- | ----------------- | ---------------------- | ---------- | -------- |
| setServerMovementEnable | NPC        | MethodDeclaration | complexMovementEnabled | Character  |          |
| serverCalculateEnable   | NPC        | SetAccessor       | complexMovementEnabled | Character  |          |

#### [删除]`<类>`Optimization 角色优化

- 删除【Optimization】类，角色优化开关作为角色设置项，替换为【AvatarSettings】中`optimizationEnabled`。

| 接口名称（老）     | 属类（老）   | 接口类型（老）    | 接口名称（新）      | 属类（新）     | 修改方案 |
| ------------------ | ------------ | ----------------- | ------------------- | -------------- | -------- |
| enableOptimization | Optimization | MethodDeclaration | optimizationEnabled | AvatarSettings |          |

#### [删除]`<类>`Particle 特效

- 原先的类名【Particle】修改为更贴近当前资源特效机制和功能的【Effect】。
- 重新梳理停止特效的接口实现，`stop()`：停止但不销毁已发射粒子；`forceStop()`：停止且销毁粒子。
- 更新资源库特效资源，暴露高级参数。支持KV形式接口对特效资源的高级参数进行设置。`setColor()`和`setColorRandom()`设置颜色或者颜色范围。`setFloat()`和`setFloatRandom()`设置数值和数值范围。`setVector()`和`setVectorRandom()`设置三维向量和向量范围。

![img](https://arkimg.ark.online/1695722088066-4.gif)

- 删除`color`修改为`maskColor`，该属性作为遮罩颜色覆盖特效整体颜色。
- 删除`particleLength`，使用`timeLength`替换。

| 接口名称（老）          | 属类（老） | 接口类型（老）      | 接口名称（新）  | 属类（新） | 修改方案 |
| ----------------------- | ---------- | ------------------- | --------------- | ---------- | -------- |
| loop                    | Particle   | GetAccessor         | loop            | Effect     | 替换     |
| loopCount               | Particle   | GetAccessor         | loopCount       | Effect     | 替换     |
| particleLength          | Particle   | GetAccessor         | timeLength      | Effect     | 替换     |
| timeLength              | Particle   | GetAccessor         | timeLength      | Effect     | 替换     |
| translucentSortPriority | Particle   | GetAccessor         | -               | Effect     | 删除     |
| color                   | Particle   | SetAccessor         | maskcolor       | Effect     | 替换     |
| loop                    | Particle   | SetAccessor         | loop            | Effect     | 替换     |
| loopCount               | Particle   | SetAccessor         | loopCount       | Effect     | 替换     |
| translucentSortPriority | Particle   | SetAccessor         | -               | Effect     | 删除     |
| onFinished              | Particle   | PropertyDeclaration | onFinish        | Effect     | 替换     |
| forceStop               | Particle   | MethodDeclaration   | forceStop       | Effect     | 替换     |
| play                    | Particle   | MethodDeclaration   | play            | Effect     | 替换     |
| setColor                | Particle   | MethodDeclaration   | setColor        | Effect     | 替换     |
| setColorRandom          | Particle   | MethodDeclaration   | setColorRandom  | Effect     | 替换     |
| setCullDistance         | Particle   | MethodDeclaration   | setCullDistance | Effect     | 替换     |
| setFloat                | Particle   | MethodDeclaration   | setFloat        | Effect     | 替换     |
| setFloatRandom          | Particle   | MethodDeclaration   | setFloatRandom  | Effect     | 替换     |
| setVector               | Particle   | MethodDeclaration   | setVector       | Effect     | 替换     |
| setVectorRandom         | Particle   | MethodDeclaration   | setVectorRandom | Effect     | 替换     |
| stop                    | Particle   | MethodDeclaration   | stop            | Effect     | 替换     |

#### [修改]`<类>`Player 玩家

- 去除继承GameObject，独立存在
- 迁移MW中散落其他命名空间的Player相关的接口进入Player类中作为静态函数使用
- 废弃玩家网络相关状态操作callback的接口，改为委托实现
- 废弃玩家账户接口，方便后续重构AcountService
- 迁移`customTimeDilation`至Pawn对象中，方便不同pawn对象使用不同时间膨胀
- 新增`control`接口和`onPawnChange`委托方便玩家切换当前的控制对象
- 新增操作玩家控制器输入旋转的接口`setControllerRotation`和`getControllerRotation`方便逐帧获取和设置当前控制器旋转输入。
- 新增创建默认角色接口`spawnDefaultCharacter`方便重新生成玩家角色对象。

| 接口名称（老）                  | 属类（老） | 接口类型（老）    | 接口名称（新）     | 属类（新） | 修改方案 |
| ------------------------------- | ---------- | ----------------- | ------------------ | ---------- | -------- |
| customTimeDilation              | Player     | GetAccessor       | customTimeDilation | Pawn       |          |
| addNetworkDisconnectListener    | Player     | MethodDeclaration | onPlayerDisconnect | Player     |          |
| addNetworkReconnectListener     | Player     | MethodDeclaration | onPlayerReconnect  | Player     |          |
| getAccount                      | Player     | MethodDeclaration | -                  |            |          |
| getPlayerID                     | Player     | MethodDeclaration | playerId           | Player     |          |
| getTeamId                       | Player     | MethodDeclaration | teamId             | Player     |          |
| getUserId                       | Player     | MethodDeclaration | userId             | Player     |          |
| getUserSystemId                 | Player     | MethodDeclaration | -                  |            |          |
| removeNetworkDisconnectListener | Player     | MethodDeclaration | onPlayerDisconnect | Player     |          |
| removeNetworkReconnectListener  | Player     | MethodDeclaration | onPlayerReconnect  | Player     |          |
| setCustomTimeDilation           | Player     | MethodDeclaration | customTimeDilation | Pawn       |          |

#### [修改]`<类>`PostProcess 后处理

- 因后处理对象并不支持叠加使用，场景中有且仅有一份生效，故027版本将**【PostProcess】后处理对象从资源库逻辑对象中删除，放置到世界对象**以默认配置存在。
- 删除继承自【GameObject】的接口，【PostProcess】**不继承【GameObject】**。
- 删除**无效&易错的接口，**仅保留**bloomIntensity**、**globalSaturation**、**globalContrast**三个全局属性。

- 新增**后处理模板**，【PostProcess】后处理对象新增`preset`属性提供7种后处理预设：
  - **默认 Default = 0**
  - **梦境 Dreamy = 1**
  - **反差色 Contrast = 2**
  - **暖阳 WarmSunshine = 3**
  - **老照片 OldPhoto = 4**
  - **夜幕 Night = 5**
  - **鲜暖色 WarmContrast = 6**

![img](https://arkimg.ark.online/1695722153743-7.webp)

- **新增【PostProcessConfig】后处理配置类**，囊括后处理当前属性，**解耦数据层和功能层**。【PostProcess】后处理对象新增`config`属性。【PostProcessConfig】保证了外部接口的纯净，此外保留了后续可能需要暴露更多复杂的后处理参数的扩展性。

| 接口名称（老）                | 属类（老）  | 接口类型（老）      | 接口名称（新） | 属类（新）  | 修改方案 |
| ----------------------------- | ----------- | ------------------- | -------------- | ----------- | -------- |
| ambientOcclusionIntensity     | PostProcess | GetAccessor         | -              |             |          |
| ambientOcclusionRadius        | PostProcess | GetAccessor         | -              |             |          |
| autoExposureBias              | PostProcess | GetAccessor         | -              |             |          |
| autoExposureMaxBrightness     | PostProcess | GetAccessor         | -              |             |          |
| autoExposureMinBrightness     | PostProcess | GetAccessor         | -              |             |          |
| bloomIntensity                | PostProcess | GetAccessor         | bloom          | PostProcess |          |
| globalContrast                | PostProcess | GetAccessor         | contrast       | PostProcess |          |
| globalGamma                   | PostProcess | GetAccessor         | -              |             |          |
| globalSaturation              | PostProcess | GetAccessor         | saturation     | PostProcess |          |
| hDRContrast                   | PostProcess | GetAccessor         | -              |             |          |
| hDRGamma                      | PostProcess | GetAccessor         | -              |             |          |
| hDRSaturation                 | PostProcess | GetAccessor         | -              |             |          |
| lDR2HDRThreshold              | PostProcess | GetAccessor         | -              |             |          |
| lDRContrast                   | PostProcess | GetAccessor         | -              |             |          |
| lDRGamma                      | PostProcess | GetAccessor         | -              |             |          |
| lDRSaturation                 | PostProcess | GetAccessor         | -              |             |          |
| lUTBlend                      | PostProcess | GetAccessor         | -              |             |          |
| lUTTextureAssetByGuid         | PostProcess | GetAccessor         | -              |             |          |
| motionBlur                    | PostProcess | GetAccessor         | -              |             |          |
| occlusionBlend                | PostProcess | GetAccessor         | -              |             |          |
| outlineCoveredAlpha           | PostProcess | GetAccessor         | -              |             |          |
| outlineCoveredEdgeAlpha       | PostProcess | GetAccessor         | -              |             |          |
| outlineNotCoveredAlpha        | PostProcess | GetAccessor         | -              |             |          |
| outlineNotCoveredEdgeAlpha    | PostProcess | GetAccessor         | -              |             |          |
| outlineWidth                  | PostProcess | GetAccessor         | -              |             |          |
| toneBlackClip                 | PostProcess | GetAccessor         | -              |             |          |
| toneCurveAmount               | PostProcess | GetAccessor         | -              |             |          |
| toneShoulder                  | PostProcess | GetAccessor         | -              |             |          |
| toneSlope                     | PostProcess | GetAccessor         | -              |             |          |
| toneToe                       | PostProcess | GetAccessor         | -              |             |          |
| toneWhiteClip                 | PostProcess | GetAccessor         | -              |             |          |
| ambientOcclusionIntensity     | PostProcess | SetAccessor         | -              |             |          |
| ambientOcclusionRadius        | PostProcess | SetAccessor         | -              |             |          |
| autoExposureBias              | PostProcess | SetAccessor         | -              |             |          |
| autoExposureMaxBrightness     | PostProcess | SetAccessor         | -              |             |          |
| autoExposureMinBrightness     | PostProcess | SetAccessor         | -              |             |          |
| bloomIntensity                | PostProcess | SetAccessor         | bloom          | PostProcess |          |
| globalContrast                | PostProcess | SetAccessor         | contrast       | PostProcess |          |
| globalGamma                   | PostProcess | SetAccessor         | -              |             |          |
| globalSaturation              | PostProcess | SetAccessor         | saturation     | PostProcess |          |
| hDRContrast                   | PostProcess | SetAccessor         | -              |             |          |
| hDRGamma                      | PostProcess | SetAccessor         | -              |             |          |
| hDRSaturation                 | PostProcess | SetAccessor         | -              |             |          |
| lDR2HDRThreshold              | PostProcess | SetAccessor         | -              |             |          |
| lDRContrast                   | PostProcess | SetAccessor         | -              |             |          |
| lDRGamma                      | PostProcess | SetAccessor         | -              |             |          |
| lDRSaturation                 | PostProcess | SetAccessor         | -              |             |          |
| lUTBlend                      | PostProcess | SetAccessor         | -              |             |          |
| lUTTextureAssetByGuid         | PostProcess | SetAccessor         | -              |             |          |
| motionBlur                    | PostProcess | SetAccessor         | -              |             |          |
| occlusionBlend                | PostProcess | SetAccessor         | -              |             |          |
| outlineCoveredAlpha           | PostProcess | SetAccessor         | -              |             |          |
| outlineCoveredEdgeAlpha       | PostProcess | SetAccessor         | -              |             |          |
| outlineNotCoveredAlpha        | PostProcess | SetAccessor         | -              |             |          |
| outlineNotCoveredEdgeAlpha    | PostProcess | SetAccessor         | -              |             |          |
| outlineWidth                  | PostProcess | SetAccessor         | -              |             |          |
| toneBlackClip                 | PostProcess | SetAccessor         | -              |             |          |
| toneCurveAmount               | PostProcess | SetAccessor         | -              |             |          |
| toneShoulder                  | PostProcess | SetAccessor         | -              |             |          |
| toneSlope                     | PostProcess | SetAccessor         | -              |             |          |
| toneToe                       | PostProcess | SetAccessor         | -              |             |          |
| toneWhiteClip                 | PostProcess | SetAccessor         | -              |             |          |
| postProcessConfig             | PostProcess | PropertyDeclaration | config         | PostProcess |          |
| addOutlineColor               | PostProcess | MethodDeclaration   | -              |             |          |
| addOutlineColor               | PostProcess | MethodDeclaration   | -              |             |          |
| getAmbientOcclusionIntensity  | PostProcess | MethodDeclaration   | -              |             |          |
| getAmbientOcclusionRadius     | PostProcess | MethodDeclaration   | -              |             |          |
| getAutoExposureBias           | PostProcess | MethodDeclaration   | -              |             |          |
| getAutoExposureMaxBrightness  | PostProcess | MethodDeclaration   | -              |             |          |
| getAutoExposureMinBrightness  | PostProcess | MethodDeclaration   | -              |             |          |
| getBloomIntensity             | PostProcess | MethodDeclaration   | bloom          | PostProcess |          |
| getConfig                     | PostProcess | MethodDeclaration   | config         | PostProcess |          |
| getGlobalContrast             | PostProcess | MethodDeclaration   | contrast       | PostProcess |          |
| getGlobalGamma                | PostProcess | MethodDeclaration   | -              |             |          |
| getGlobalSaturation           | PostProcess | MethodDeclaration   | saturation     | PostProcess |          |
| getHDRContrast                | PostProcess | MethodDeclaration   | -              |             |          |
| getHDRGamma                   | PostProcess | MethodDeclaration   | -              |             |          |
| getHDRSaturation              | PostProcess | MethodDeclaration   | -              |             |          |
| getInstance                   | PostProcess | MethodDeclaration   | -              |             |          |
| getLDR2HDRThreshold           | PostProcess | MethodDeclaration   | -              |             |          |
| getLDRContrast                | PostProcess | MethodDeclaration   | -              |             |          |
| getLDRGamma                   | PostProcess | MethodDeclaration   | -              |             |          |
| getLDRSaturation              | PostProcess | MethodDeclaration   | -              |             |          |
| getLUTBlend                   | PostProcess | MethodDeclaration   | -              |             |          |
| getLUTTextureAssetByGuid      | PostProcess | MethodDeclaration   | -              |             |          |
| getMotionBlur                 | PostProcess | MethodDeclaration   | -              |             |          |
| getOcclusionBlend             | PostProcess | MethodDeclaration   | -              |             |          |
| getOutlineCoveredAlpha        | PostProcess | MethodDeclaration   | -              |             |          |
| getOutlineCoveredEdgeAlpha    | PostProcess | MethodDeclaration   | -              |             |          |
| getOutlineNotCoveredAlpha     | PostProcess | MethodDeclaration   | -              |             |          |
| getOutlineNotCoveredEdgeAlpha | PostProcess | MethodDeclaration   | -              |             |          |
| getOutlineWidth               | PostProcess | MethodDeclaration   | -              |             |          |
| getToneBlackClip              | PostProcess | MethodDeclaration   | -              |             |          |
| getToneCurveAmount            | PostProcess | MethodDeclaration   | -              |             |          |
| getToneShoulder               | PostProcess | MethodDeclaration   | -              |             |          |
| getToneSlope                  | PostProcess | MethodDeclaration   | -              |             |          |
| getToneToe                    | PostProcess | MethodDeclaration   | -              |             |          |
| getToneWhiteClip              | PostProcess | MethodDeclaration   | -              |             |          |
| setAmbientOcclusionIntensity  | PostProcess | MethodDeclaration   | -              |             |          |
| setAmbientOcclusionRadius     | PostProcess | MethodDeclaration   | -              |             |          |
| setAutoExposureBias           | PostProcess | MethodDeclaration   | -              |             |          |
| setAutoExposureMaxBrightness  | PostProcess | MethodDeclaration   | -              |             |          |
| setAutoExposureMinBrightness  | PostProcess | MethodDeclaration   | -              |             |          |
| setBloomIntensity             | PostProcess | MethodDeclaration   | bloom          | PostProcess |          |
| setConfig                     | PostProcess | MethodDeclaration   | config         | PostProcess |          |
| setGlobalContrast             | PostProcess | MethodDeclaration   | contrast       | PostProcess |          |
| setGlobalGamma                | PostProcess | MethodDeclaration   | -              |             |          |
| setGlobalSaturation           | PostProcess | MethodDeclaration   | saturation     | PostProcess |          |
| setHDRContrast                | PostProcess | MethodDeclaration   | -              |             |          |
| setHDRGamma                   | PostProcess | MethodDeclaration   | -              |             |          |
| setHDRSaturation              | PostProcess | MethodDeclaration   | -              |             |          |
| setLDR2HDRThreshold           | PostProcess | MethodDeclaration   | -              |             |          |
| setLDRContrast                | PostProcess | MethodDeclaration   | -              |             |          |
| setLDRGamma                   | PostProcess | MethodDeclaration   | -              |             |          |
| setLDRSaturation              | PostProcess | MethodDeclaration   | -              |             |          |
| setLUTBlend                   | PostProcess | MethodDeclaration   | -              |             |          |
| setLUTTextureAssetByGuid      | PostProcess | MethodDeclaration   | -              |             |          |
| setMotionBlur                 | PostProcess | MethodDeclaration   | -              |             |          |
| setOcclusionBlend             | PostProcess | MethodDeclaration   | -              |             |          |
| setOutlineCoveredAlpha        | PostProcess | MethodDeclaration   | -              |             |          |
| setOutlineCoveredEdgeAlpha    | PostProcess | MethodDeclaration   | -              |             |          |
| setOutlineNotCoveredAlpha     | PostProcess | MethodDeclaration   | -              |             |          |
| setOutlineNotCoveredEdgeAlpha | PostProcess | MethodDeclaration   | -              |             |          |
| setOutlineWidth               | PostProcess | MethodDeclaration   | -              |             |          |
| setPreset                     | PostProcess | MethodDeclaration   | preset         | PostProcess |          |
| setToneBlackClip              | PostProcess | MethodDeclaration   | -              |             |          |
| setToneCurveAmount            | PostProcess | MethodDeclaration   | -              |             |          |
| setToneShoulder               | PostProcess | MethodDeclaration   | -              |             |          |
| setToneSlope                  | PostProcess | MethodDeclaration   | -              |             |          |
| setToneToe                    | PostProcess | MethodDeclaration   | -              |             |          |
| setToneWhiteClip              | PostProcess | MethodDeclaration   | -              |             |          |

#### [修改]`<类>`PostProcessConfig 后处理配置类

- 后续需要暴露更多复杂的后处理参数即在配置类中新增。

| 接口名称（老）             | 属类（老）        | 接口类型（老）      | 接口名称（新）   | 属类（新）        | 修改方案 |
| -------------------------- | ----------------- | ------------------- | ---------------- | ----------------- | -------- |
| ambientOcclusionIntensity  | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| ambientOcclusionRadius     | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| autoExposureBias           | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| autoExposureMaxBrightness  | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| autoExposureMinBrightness  | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| bloomIntensity             | PostProcessConfig | PropertyDeclaration | bloomIntensity   | PostProcessConfig |          |
| globalContrast             | PostProcessConfig | PropertyDeclaration | globalContrast   | PostProcessConfig |          |
| globalGamma                | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| globalSaturation           | PostProcessConfig | PropertyDeclaration | globalSaturation | PostProcessConfig |          |
| hdrContrast                | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| hdrGamma                   | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| hdrSaturation              | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| ldr2HDRThreshold           | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| ldrContrast                | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| ldrGamma                   | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| ldrSaturation              | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| lutBlend                   | PostProcessConfig | PropertyDeclaration | lutBlend         | PostProcessConfig |          |
| motionBlur                 | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| occluderBlend              | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| outlineCoveredAlpha        | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| outlineCoveredEdgeAlpha    | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| outlineNotCoveredAlpha     | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| outlineNotCoveredEdgeAlpha | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| outlineWidth               | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| toneBlackClip              | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| toneCurveAmount            | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| toneShoulder               | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| toneSlope                  | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| toneToe                    | PostProcessConfig | PropertyDeclaration | -                |                   |          |
| toneWhiteClip              | PostProcessConfig | PropertyDeclaration | -                |                   |          |

#### [删除]`<类>`Projectile 投掷物

- 删除【Projectile】投掷物，**替换为重构对象【ProjectileMovement】**投掷物移动功能类。
- 删除继承自【GameObject】的接口，【ProjectileMovement】**不继承【GameObject】**。以后使用投掷功能不再需要生成【GameObject】，再挂载具体的对象。而是将它转化成带有投掷能力的对象自行发射。
- 删除碰撞胶囊体范围属性`collisionLength`和`collisionRadius`，**【ProjectileMovement】的碰撞形状和范围由“被投掷对象”的【简单碰撞信息】决定。**
- 删除飞行距离属性`flyRange`，因为它仅在无重力且不反弹时生效。而【ProjectileMovement】的运动距离由一系列复杂的运动参数（重力、碰撞速度损失系数、初始速度、加速度等）共同决定。因此**替换为****`LifeSpan`****运动时间供用户自定义。**
- 删除最大弹跳次数`maxBounceCount`，因为弹跳次数是结果而不是运动因数。替换为碰撞速度损失系数`speedRetention`。**此外【ProjectileMovement】的碰撞结果：反弹|穿透，由“被投掷对象”的碰撞状态****`collisionStatus`****决定。**开启碰撞或者碰撞响应为block则反弹，其余皆为穿透（如果关闭碰撞会无法触发委托）。
- 删除`onProjectileBeginOverlap`“开始重叠”、`onProjectileBounce`“反弹”、`onProjectileEndOverlap`“结束重叠”、`onProjectileHit`“击中”，将四个委托事件合并，**统一使用****`onProjectileHit`****击中事件委托。**重叠、反弹和击中只是碰撞结果的不同，触发同一个事件。
- 删除`onProjectileInterrupt`“投掷物自动终止”，替换为`onProjectileLifeEnd`“投掷物生命结束”。
- 删除绑定玩家方法`bindPlayer`，**【ProjectileMovement】不依赖具体的玩家工作。**此外提供owner属性方便用户指定发射人后续用作业务逻辑。
- 删除统一赋值的`init`方法，新增【ProjectileMovementConfig】投掷物配置类，囊括投掷物属性，解耦数据层和功能层。**【ProjectileMovement】的构造方法中新增config参数便于用户使用配置去构造投掷物**。

| 接口名称（老）           | 属类（老） | 接口类型（老）      | 接口名称（新）      | 属类（新）         | 修改方案 |
| ------------------------ | ---------- | ------------------- | ------------------- | ------------------ | -------- |
| collisionLength          | Projectile | GetAccessor         | -                   |                    |          |
| collisionRadius          | Projectile | GetAccessor         | -                   |                    |          |
| flyRange                 | Projectile | GetAccessor         | -                   |                    |          |
| gravityScale             | Projectile | GetAccessor         | gravityScale        | ProjectileMovement |          |
| initialSpeed             | Projectile | GetAccessor         | initialSpeed        | ProjectileMovement |          |
| maxBounceCount           | Projectile | GetAccessor         | -                   |                    |          |
| simulatePhysics          | Projectile | GetAccessor         | -                   |                    |          |
| collisionLength          | Projectile | SetAccessor         | -                   |                    |          |
| collisionRadius          | Projectile | SetAccessor         | -                   |                    |          |
| flyRange                 | Projectile | SetAccessor         | -                   |                    |          |
| gravityScale             | Projectile | SetAccessor         | gravityScale        | ProjectileMovement |          |
| initialSpeed             | Projectile | SetAccessor         | initialSpeed        | ProjectileMovement |          |
| maxBounceCount           | Projectile | SetAccessor         | -                   |                    |          |
| simulatePhysics          | Projectile | SetAccessor         | -                   |                    |          |
| onProjectileBeginOverlap | Projectile | PropertyDeclaration | onProjectileHit     | ProjectileMovement |          |
| onProjectileBounce       | Projectile | PropertyDeclaration | onProjectileHit     | ProjectileMovement |          |
| onProjectileEndOverlap   | Projectile | PropertyDeclaration | onProjectileHit     | ProjectileMovement |          |
| onProjectileHit          | Projectile | PropertyDeclaration | onProjectileHit     | ProjectileMovement |          |
| onProjectileInterrupt    | Projectile | PropertyDeclaration | onProjectileLifeEnd | ProjectileMovement |          |
| bindPlayer               | Projectile | MethodDeclaration   | -                   |                    |          |
| init                     | Projectile | MethodDeclaration   | -                   |                    |          |
| launch                   | Projectile | MethodDeclaration   | launch              | ProjectileMovement |          |
| pause                    | Projectile | MethodDeclaration   | pause               | ProjectileMovement |          |
| resume                   | Projectile | MethodDeclaration   | resume              | ProjectileMovement |          |

#### [删除]`<类>`ProjectileLauncher 投掷物发射器

- 删除【ProjectileLauncher】投掷发射器，**替换为重构对象【ObjectLauncher】对象发射器**。
- **删除对象中过度设计**的接口：
  - **加速**：`accelerationEnable`、`accelerationEnableDistance`、`accelerationEnableMode`、`accelerationEnableTime`。
  - **重力**：`gravityEnable`、`gravityEnableDistance`、`gravityEnableMode`、`gravityEnableTime`。
  - **发射参数**：`startLocation`、`launchDirection`。
  - **生命周期**：`maxCollisionTimes`、`isAutoDestroy`、`range`
  - **绘制**：`traceLineStyle`、`drawPredictedTrajectory`
- 删除碰撞模式属性`collisionMode`，替换为布尔值属性`isShouldBounce`，true为反弹，false为穿透。
- 删除碰撞损失因数属性`collisionLossCoefficient`，替换为`collisionVelocityRetention`碰撞速度保留比例。
- 投掷物形状由球体->胶囊体。因此删除检测半径`detectionRadius`，替换为胶囊体半径`capsuleRadius`和`capsuleHalfLength`。
- 删除重力加速度`gravitationalAcceleration`，替换为重力缩放`gravityScale`，采用世界（物理区域）重力值。
- 发射器在发射时制造投掷物，对应委托事件`onProjectileSpawned`替换为`onProjectileLifeStart`，当投掷物不再运动时会自动销毁，对应委托事件onProjectileDestroy替换为`onProjectileLifeEnd`
- 删除绑定/解绑玩家方法`bindPlayer`和`unbindPlayer`，【**ObjectLauncher**】不再需要手动绑定Player，当客户端调用发射方法时，发射器会给制造的投掷物实例中owner属性赋值该客户端便于用户执行游戏逻辑。在服务端调用发射方法时owner为空。
- 新增【ProjectileInst】投掷物实例，发射器发射对象时流程如下：
  - 通过自身参数创建一个投掷物实例。
  - 根据调用端给投掷物实例owner属性赋值。
  - 将传入的发射对象挂载到投掷物实例上。
  - 投掷物实例开始运动。
  - 投掷物实例触发发射器对应事件执行绑定函数。
  - 投掷物停止运动，解绑子对象后自行销毁。

| 接口名称（老）                        | 属类（老）         | 接口类型（老）      | 接口名称（新）                        | 属类（新）     | 修改方案 |
| ------------------------------------- | ------------------ | ------------------- | ------------------------------------- | -------------- | -------- |
| acceleration                          | ProjectileLauncher | GetAccessor         | acceleration                          | ObjectLauncher |          |
| accelerationEnable                    | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| accelerationEnableDistance            | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| accelerationEnableMode                | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| accelerationEnableTime                | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| collisionLossCoefficient              | ProjectileLauncher | GetAccessor         | collisionVelocityRetention            | ObjectLauncher |          |
| collisionMode                         | ProjectileLauncher | GetAccessor         | isShouldBounce                        | ObjectLauncher |          |
| detectionRadius                       | ProjectileLauncher | GetAccessor         | capsuleRadius                         | ObjectLauncher |          |
| gravitationalAcceleration             | ProjectileLauncher | GetAccessor         | gravityScale                          | ObjectLauncher |          |
| gravityEnable                         | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| gravityEnableDistance                 | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| gravityEnableMode                     | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| gravityEnableTime                     | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| initialSpeed                          | ProjectileLauncher | GetAccessor         | initialSpeed                          | ObjectLauncher |          |
| isAutoDestroy                         | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| launchDirection                       | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| lifeSpan                              | ProjectileLauncher | GetAccessor         | lifeSpan                              | ObjectLauncher |          |
| maxCollisionTimes                     | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| maxSpeed                              | ProjectileLauncher | GetAccessor         | maxSpeed                              | ObjectLauncher |          |
| range                                 | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| startLocation                         | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| traceLineStyle                        | ProjectileLauncher | GetAccessor         | -                                     |                |          |
| acceleration                          | ProjectileLauncher | SetAccessor         | acceleration                          | ObjectLauncher |          |
| accelerationEnable                    | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| accelerationEnableDistance            | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| accelerationEnableMode                | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| accelerationEnableTime                | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| collisionLossCoefficient              | ProjectileLauncher | SetAccessor         | collisionVelocityRetention            | ObjectLauncher |          |
| collisionMode                         | ProjectileLauncher | SetAccessor         | isShouldBounce                        | ObjectLauncher |          |
| detectionRadius                       | ProjectileLauncher | SetAccessor         | capsuleRadius                         | ObjectLauncher |          |
| gravitationalAcceleration             | ProjectileLauncher | SetAccessor         | gravityScale                          | ObjectLauncher |          |
| gravityEnable                         | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| gravityEnableDistance                 | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| gravityEnableMode                     | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| gravityEnableTime                     | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| initialSpeed                          | ProjectileLauncher | SetAccessor         | initialSpeed                          | ObjectLauncher |          |
| isAutoDestroy                         | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| launchDirection                       | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| lifeSpan                              | ProjectileLauncher | SetAccessor         | lifeSpan                              | ObjectLauncher |          |
| maxCollisionTimes                     | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| maxSpeed                              | ProjectileLauncher | SetAccessor         | maxSpeed                              | ObjectLauncher |          |
| range                                 | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| startLocation                         | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| traceLineStyle                        | ProjectileLauncher | SetAccessor         | -                                     |                |          |
| onProjectileDestroy                   | ProjectileLauncher | PropertyDeclaration | onProjectileLifeEnd                   | ObjectLauncher |          |
| onProjectileHit                       | ProjectileLauncher | PropertyDeclaration | onProjectileHit                       | ObjectLauncher |          |
| onProjectileSpawned                   | ProjectileLauncher | PropertyDeclaration | onProjectileLifeStart                 | ObjectLauncher |          |
| bindPlayer                            | ProjectileLauncher | MethodDeclaration   | -                                     |                |          |
| drawPredictedTrajectory               | ProjectileLauncher | MethodDeclaration   | -                                     |                |          |
| predictedTrajectory                   | ProjectileLauncher | MethodDeclaration   | predictedTrajectory                   | ObjectLauncher |          |
| spawnProjectileInstanceLaunch         | ProjectileLauncher | MethodDeclaration   | spawnProjectileInstanceLaunch         | ObjectLauncher |          |
| spawnProjectileInstanceLaunchToTarget | ProjectileLauncher | MethodDeclaration   | spawnProjectileInstanceLaunchToTarget | ObjectLauncher |          |
| unbindPlayer                          | ProjectileLauncher | MethodDeclaration   | -                                     |                |          |

#### [修改]`<类>`PurchaseService 购买服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。

| 接口名称（老） | 属类（老）      | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | --------------- | ----------------- | -------------- | ---------- | -------- |
| getInstance    | PurchaseService | MethodDeclaration | -              |            |          |

#### [修改]`<类>`RoomService 购买服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。
- 删除无效和MGS相关接口，以及部分内部方法。

| 接口名称（老）           | 属类（老）  | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| ------------------------ | ----------- | ----------------- | -------------- | ---------- | -------- |
| createAndJoinRoom        | RoomService | MethodDeclaration | -              |            |          |
| destroySDK               | RoomService | MethodDeclaration | -              |            |          |
| dispatchMGSChatMessage   | RoomService | MethodDeclaration | -              |            |          |
| getInstance              | RoomService | MethodDeclaration | -              |            |          |
| initAndLoginMGS          | RoomService | MethodDeclaration | -              |            |          |
| invokeMGSConfig          | RoomService | MethodDeclaration | -              |            |          |
| joinRoom                 | RoomService | MethodDeclaration | -              |            |          |
| leaveRoom                | RoomService | MethodDeclaration | -              |            |          |
| queryPlayerAction        | RoomService | MethodDeclaration | -              |            |          |
| registerMGSEvent         | RoomService | MethodDeclaration | -              |            |          |
| registerMGSEventListener | RoomService | MethodDeclaration | -              |            |          |
|                          |             | MethodDeclaration | -              |            |          |

#### [修改]`<类>`RouteService 跳游戏服务

- 删除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。

| 接口名称（老） | 属类（老）   | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ------------ | ----------------- | -------------- | ---------- | -------- |
| getInstance    | RouteService | MethodDeclaration | -              |            |          |

#### [删除]`<类>`SkyBox 天空盒

- **删除【SkyBox】天空盒，接口修改为静态调用形式迁移至新增对象【Skybox】**。因为天空盒并不支持叠加使用，场景中有且仅有一份生效，故027版本将**其放置到世界对象**以默认配置存在。
- 删除继承自【GameObject】的接口，**【Skybox】不继承【GameObject】**。
- 对属性名，方法名进行修改，删除老接口，替换为新接口。

| 接口名称（老）           | 属类（老） | 接口类型（老）    | 接口名称（新）           | 属类（新） | 修改方案 |
| ------------------------ | ---------- | ----------------- | ------------------------ | ---------- | -------- |
| cloudDensity             | SkyBox     | GetAccessor       | cloudDensity             | Skybox     |          |
| cloudEnable              | SkyBox     | GetAccessor       | cloudVisible             | Skybox     |          |
| cloudEnabled             | SkyBox     | GetAccessor       | cloudVisible             | Skybox     |          |
| cloudOpacity             | SkyBox     | GetAccessor       | cloudOpacity             | Skybox     |          |
| cloudSpeed               | SkyBox     | GetAccessor       | cloudSpeed               | Skybox     |          |
| cloudTint                | SkyBox     | GetAccessor       | cloudColor               | Skybox     |          |
| moonEnable               | SkyBox     | GetAccessor       | moonVisible              | Skybox     |          |
| moonIntensity            | SkyBox     | GetAccessor       | moonIntensity            | Skybox     |          |
| moonSize                 | SkyBox     | GetAccessor       | moonSize                 | Skybox     |          |
| moonTint                 | SkyBox     | GetAccessor       | moonColor                | Skybox     |          |
| skyDomeBotTint           | SkyBox     | GetAccessor       | skyDomeBottomColor       | Skybox     |          |
| skyDomeGradientEnable    | SkyBox     | GetAccessor       | skyDomeGradientEnabled   | Skybox     |          |
| skyDomeHorizontalFallOff | SkyBox     | GetAccessor       | skyDomeHorizontalFallOff | Skybox     |          |
| skyDomeHorizontalTint    | SkyBox     | GetAccessor       | skyDomeMiddleColor       | Skybox     |          |
| skyDomeIntensity         | SkyBox     | GetAccessor       | skyDomeIntensity         | Skybox     |          |
| skyDomeTint              | SkyBox     | GetAccessor       | skyDomeBaseColor         | Skybox     |          |
| skyDomeTopTint           | SkyBox     | GetAccessor       | skyDomeTopColor          | Skybox     |          |
| skyPreset                | SkyBox     | GetAccessor       | preset                   | Skybox     |          |
| starEnable               | SkyBox     | GetAccessor       | starVisible              | Skybox     |          |
| starIntensity            | SkyBox     | GetAccessor       | starIntensity            | Skybox     |          |
| starTiling               | SkyBox     | GetAccessor       | starDensity              | Skybox     |          |
| sunEnable                | SkyBox     | GetAccessor       | sunVisible               | Skybox     |          |
| sunIntensity             | SkyBox     | GetAccessor       | sunIntensity             | Skybox     |          |
| sunSize                  | SkyBox     | GetAccessor       | sunSize                  | Skybox     |          |
| sunTint                  | SkyBox     | GetAccessor       | sunColor                 | Skybox     |          |
| cloudDensity             | SkyBox     | SetAccessor       | cloudDensity             | Skybox     |          |
| cloudEnable              | SkyBox     | SetAccessor       | cloudVisible             | Skybox     |          |
| cloudOpacity             | SkyBox     | SetAccessor       | cloudOpacity             | Skybox     |          |
| cloudSpeed               | SkyBox     | SetAccessor       | cloudSpeed               | Skybox     |          |
| cloudTextureAssetByID    | SkyBox     | SetAccessor       | cloudTextureID           | SkyBox     |          |
| cloudTint                | SkyBox     | SetAccessor       | cloudColor               | Skybox     |          |
| moonEnable               | SkyBox     | SetAccessor       | moonVisible              | Skybox     |          |
| moonIntensity            | SkyBox     | SetAccessor       | moonIntensity            | Skybox     |          |
| moonSize                 | SkyBox     | SetAccessor       | moonSize                 | Skybox     |          |
| moonTextureAssetByID     | SkyBox     | SetAccessor       | moonTextureID            | SkyBox     |          |
| moonTint                 | SkyBox     | SetAccessor       | moonColor                | Skybox     |          |
| skyDomeBotTint           | SkyBox     | SetAccessor       | skyDomeBottomColor       | Skybox     |          |
| skyDomeGradientEnable    | SkyBox     | SetAccessor       | skyDomeGradientEnabled   | Skybox     |          |
| skyDomeHorizontalFallOff | SkyBox     | SetAccessor       | skyDomeHorizontalFallOff | Skybox     |          |
| skyDomeHorizontalTint    | SkyBox     | SetAccessor       | skyDomeMiddleColor       | Skybox     |          |
| skyDomeIntensity         | SkyBox     | SetAccessor       | skyDomeIntensity         | Skybox     |          |
| skyDomeTextureAssetByID  | SkyBox     | SetAccessor       | skyDomeTextureID         | SkyBox     |          |
| skyDomeTint              | SkyBox     | SetAccessor       | skyDomeBaseColor         | Skybox     |          |
| skyDomeTopTint           | SkyBox     | SetAccessor       | skyDomeTopColor          | Skybox     |          |
| skyPreset                | SkyBox     | SetAccessor       | preset                   | Skybox     |          |
| starEnable               | SkyBox     | SetAccessor       | starVisible              | Skybox     |          |
| starIntensity            | SkyBox     | SetAccessor       | starIntensity            | Skybox     |          |
| starTextureAssetByID     | SkyBox     | SetAccessor       | starTextureID            | SkyBox     |          |
| starTiling               | SkyBox     | SetAccessor       | starDensity              | Skybox     |          |
| sunEnable                | SkyBox     | SetAccessor       | sunVisible               | Skybox     |          |
| sunIntensity             | SkyBox     | SetAccessor       | sunIntensity             | Skybox     |          |
| sunSize                  | SkyBox     | SetAccessor       | sunSize                  | Skybox     |          |
| sunTextureAssetByID      | SkyBox     | SetAccessor       | sunTextureID             | SkyBox     |          |
| sunTint                  | SkyBox     | SetAccessor       | sunColor                 | Skybox     |          |
| refresh                  | SkyBox     | MethodDeclaration | refresh                  | Skybox     |          |
| reset                    | SkyBox     | MethodDeclaration | reset                    | Skybox     |          |

#### [修改]`<类>`SkyLight 天空光

- 删除【DirectionalLight】平行光，接口修改为静态调用形式迁移至新增对象【Lighting】。
- 删除继承自【GameObject】的接口，【Lighting】不继承【GameObject】。
- 删除光强`intensity`替换为`brightness`。

| 接口名称（老） | 属类（老） | 接口类型（老） | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ---------- | -------------- | -------------- | ---------- | -------- |
| intensity      | SkyLight   | GetAccessor    | brightness     | Lighting   |          |
| lightColor     | SkyLight   | GetAccessor    | lightColor     | Lighting   |          |
| intensity      | SkyLight   | SetAccessor    | brightness     | Lighting   |          |
| lightColor     | SkyLight   | SetAccessor    | lightColor     | Lighting   |          |

#### [删除]`<类>`SomatotypeBase 体型基类

- 删除【SomatotypeBase】体型基类。内含的角色mesh描边接口使用新增的【Pawn】对象中代替。
- 删除`enableOutline`和`postProcessObj`，因后处理对象已修改，接口无效。
- 删除启用后处理方法`enablePostProcess`，现在场景中默认存在一个【PostProcess】对象，无需启用。
- 修改普通描边`setOutline`和后处理描边`setPostProcessOutline`实现，方法中参数控制描边开启，颜色和宽度。

| 接口名称（老）    | 属类（老）     | 接口类型（老）      | 接口名称（新）        | 属类（新） | 修改方案 |
| ----------------- | -------------- | ------------------- | --------------------- | ---------- | -------- |
| enableOutline     | SomatotypeBase | PropertyDeclaration | -                     |            |          |
| postProcessObj    | SomatotypeBase | PropertyDeclaration | -                     |            |          |
| enablePostProcess | SomatotypeBase | MethodDeclaration   | -                     |            | 修改方案 |
| setOutline        | SomatotypeBase | MethodDeclaration   | setOutline            | Pawn       |          |
| setOutlineAdvance | SomatotypeBase | MethodDeclaration   | setPostProcessOutline | Pawn       |          |

#### [修改]`<类>`Sound 音效

- 新增音效衰减形状，支持球体，胶囊体，盒体和锥体。
- 新增`attenuationShapeExtents`属性统一表示音效形状尺寸，删除原有尺寸相关接口。
- 新增衰减方式`attenuationDistanceModel`，提供线性、指数、倒数、反指数四种衰减函数曲线。
- 废弃失效接口：`drawInnerBounds`和`getIsDrawInnerBounds()`。
- 删除失效接口：`setSoundSphere()`。
- 删除`autoPlay`这种仅编辑器可见的特例属性，避免歧义。
- 删除`setAudioAssetByGuid()`，新增`setSoundAsset()`代替。

| 接口名称（老）       | 属类（老） | 接口类型（老）      | 接口名称（新）          | 属类（新） | 修改方案 |
| -------------------- | ---------- | ------------------- | ----------------------- | ---------- | -------- |
| autoPlay             | Sound      | GetAccessor         | -                       |            |          |
| currentProgress      | Sound      | GetAccessor         | timePosition            | Sound      |          |
| drawInnerBounds      | Sound      | GetAccessor         | -                       |            |          |
| duration             | Sound      | GetAccessor         | timeLength              | Sound      |          |
| fallOffDistance      | Sound      | GetAccessor         | falloffDistance         | Sound      |          |
| innerRadius          | Sound      | GetAccessor         | attenuationShapeExtents | Sound      |          |
| isAutoPlay           | Sound      | GetAccessor         | -                       |            |          |
| loop                 | Sound      | GetAccessor         | isLoop                  | Sound      |          |
| outerRadius          | Sound      | GetAccessor         | falloffDistance         | Sound      |          |
| playState            | Sound      | GetAccessor         | -                       |            |          |
| shapeExtents         | Sound      | GetAccessor         | attenuationShapeExtents | Sound      |          |
| soundDistance        | Sound      | GetAccessor         | attenuationShapeExtents | Sound      |          |
| spatialization       | Sound      | GetAccessor         | isSpatialization        | Sound      |          |
| timelength           | Sound      | GetAccessor         | timeLength              | Sound      |          |
| uiSound              | Sound      | GetAccessor         | isUISound               | Sound      |          |
| volumeMultiplier     | Sound      | GetAccessor         | volume                  | Sound      |          |
| audioAsset           | Sound      | SetAccessor         | setSoundAsset           | Sound      |          |
| autoPlay             | Sound      | SetAccessor         | timeLength              | Sound      |          |
| drawInnerBounds      | Sound      | SetAccessor         | -                       |            |          |
| fallOffDistance      | Sound      | SetAccessor         | falloffDistance         | Sound      |          |
| innerRadius          | Sound      | SetAccessor         | attenuationShapeExtents | Sound      |          |
| isAutoPlay           | Sound      | SetAccessor         | -                       |            |          |
| loop                 | Sound      | SetAccessor         | isLoop                  | Sound      |          |
| outerRadius          | Sound      | SetAccessor         | falloffDistance         | Sound      |          |
| shapeExtents         | Sound      | SetAccessor         | attenuationShapeExtents | Sound      |          |
| soundDistance        | Sound      | SetAccessor         | attenuationShapeExtents | Sound      |          |
| spatialization       | Sound      | SetAccessor         | isSpatialization        | Sound      |          |
| uiSound              | Sound      | SetAccessor         | isUISound               | Sound      |          |
| volumeMultiplier     | Sound      | SetAccessor         | volume                  | Sound      |          |
| onSoundFinished      | Sound      | PropertyDeclaration | onFinish                | Sound      |          |
| onSoundPaused        | Sound      | PropertyDeclaration | onPause                 | Sound      |          |
| onSoundStarted       | Sound      | PropertyDeclaration | onPlay                  | Sound      |          |
| getIsDrawInnerBounds | Sound      | MethodDeclaration   | -                       |            |          |
| setAudioAssetByGuid  | Sound      | MethodDeclaration   | setSoundAsset           | Sound      |          |
| setSoundSphere       | Sound      | MethodDeclaration   | -                       |            |          |

#### [删除]`<类>`Sequence 缓动序列

删除【Sequence 】，替换为【TweenSequence】。

| 接口名称（老） | 属类（老） | 接口类型（老）    | 接口名称（新） | 属类（新）    | 修改方案 |
| -------------- | ---------- | ----------------- | -------------- | ------------- | -------- |
| nextID         | Sequence   | MethodDeclaration | nextID         | TweenSequence |          |

#### [修改]`<类>`SoundService 音效服务

- 去除`getInstance()`，支持类名+方法名的调用方式，缩短调用链。
- 删除`clearAll`方法，屏蔽用户对内部对象池的操作。

| 接口名称（老）       | 属类（老）   | 接口类型（老）    | 接口名称（新） | 属类（新）   | 修改方案 |
| -------------------- | ------------ | ----------------- | -------------- | ------------ | -------- |
| clearAll             | SoundService | MethodDeclaration | -              |              |          |
| get3DSoundGameObject | SoundService | MethodDeclaration | get3DSoundById | SoundService |          |
| getInstance          | SoundService | MethodDeclaration | -              |              |          |

#### [修改]`<类>`Stance 基础姿态

删除内部接口`playInternal`和`stopInternal`。

| 接口名称（老） | 属类（老） | 接口类型（老） | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | ---------- | -------------- | -------------- | ---------- | -------- |
| playInternal   | Stance     | -              |                |            |          |
| stopInternal   | Stance     | -              |                |            |          |

#### [修改]`<类>`SwimmingVolume 游泳区

- 删除`inArea`方法，改为直接判断角色对象游泳状态
- 新增`fluidFriction`流体摩擦力属性

| 接口名称（老） | 属类（老）     | 接口类型（老） | 接口名称（新） | 属类（新） | 修改方案 |
| -------------- | -------------- | -------------- | -------------- | ---------- | -------- |
| inArea         | SwimmingVolume | -              |                |            | 修改方案 |

#### [修改]`<命名空间>`SelectUtil 选择工具

- 由命名空间修改为类对象，静态封装原有方法，类名仍采用【SelectUtil】。 

#### [修改]`<类>`Trigger 触发器

- 新增`shape`属性替换切换形状和判断形状的接口，`shapeExtent`属性控制形状大小。
- 新增`enabled`替换设置触发器激活状态的接口。

| 接口名称（老）      | 属类（老） | 接口类型（老）    | 接口名称（新） | 属类（新） | 修改方案 |
| ------------------- | ---------- | ----------------- | -------------- | ---------- | -------- |
| isBoxShape          | Trigger    | MethodDeclaration | shape          | Trigger    |          |
| isInArea            | Trigger    | MethodDeclaration | checkInArea    | Trigger    |          |
| isSphereShape       | Trigger    | MethodDeclaration | shape          | Trigger    |          |
| setBoxExtent        | Trigger    | MethodDeclaration | shapeExtent    | Trigger    |          |
| setCollisionEnabled | Trigger    | MethodDeclaration | enabled        | Trigger    |          |
| setSphereRadius     | Trigger    | MethodDeclaration | shapeExtent    | Trigger    |          |
| toggleTriggerShape  | Trigger    | MethodDeclaration | shape          | Trigger    |          |

#### [修改]`<类>`UIWidget 世界UI

| 接口名称（老）   | 属类（老） | 接口类型（老）    | 接口名称（新）    | 属类（新） | 修改方案 |
| ---------------- | ---------- | ----------------- | ----------------- | ---------- | -------- |
| cylinderArcAngle | UIWidget   | GetAccessor       | -                 |            |          |
| geometryMode     | UIWidget   | GetAccessor       | -                 |            |          |
| cylinderArcAngle | UIWidget   | SetAccessor       | -                 |            |          |
| geometryMode     | UIWidget   | SetAccessor       | -                 |            |          |
| getUI            | UIWidget   | MethodDeclaration | getTargetUIWidget | UIWidget   |          |
| setUI            | UIWidget   | MethodDeclaration | setTargetUIWidget | UIWidget   |          |
| setUIbyGUID      | UIWidget   | MethodDeclaration | setUIbyID         | UIWidget   |          |

#### [删除]`<类>`VehicleCameraSetting 载具摄像机配置

因摄像机系统重构，删除了摄像机配置相关接口，【VehicleCameraSetting】 载具摄像机配置同样进行删除处理。

#### [新增]`<类>`Navigation 寻路（导航）

- 新增【Navigation】寻路类，承载寻路相关接口。

| API含义  | 老API                  | 新API                       | 修改类型       |
| -------- | ---------------------- | --------------------------- | -------------- |
| 跟随目标 | Gameplay.follow()      | Navigation.follow()         | 使用新接口代替 |
| 停止跟随 | Gameplay.clearFollow() | Navigation.clearFollow()    | 使用新接口代替 |
| 寻路移动 | Gameplay.moveTo()      | Navigation.navigateTo()     | 使用新接口代替 |
| 导航停止 | Gameplay.clearMoveTo() | Navigation.stopNavigateTo() | 使用新接口代替 |

#### [新增]`<类>`Pawn 控制对象

- 新增【Pawn】对象，保留后续可操作对象增加的扩展性，便于Player与Character解耦。
- pass描边和后处理描边由其他命名空间下沉至具体Pawn对象中，直接调用。
- 自定义时间膨胀由Player迁移至Pawn对象中，方便独立生效。

#### [删除]`<类>`PlayerStart 初生点

#### [删除]`<类>`Prefab 预制体

#### [废弃]`<类>`UGCService UGC服务

删除外部内部使用对象【UGCService】

