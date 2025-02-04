# 游戏版本及实验

::: tip 阅读本文大概需要15分钟

本文将帮助您上手游戏版本管理功能，了解什么是AB实验，通过版本管理游戏上下线及版本实验。

::: 

您每次点击游戏发布&更新，都会形成一个游戏版本，可在【发布管理】-【版本管理】查看每个游戏的版本列表，版本管理支持您进行游戏上下线、游戏版本实验及版本的回滚操作等。

![CleanShot 2023-09-25 at 11.05.28](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.05.28.webp)

## 版本管理

### 游戏发布设置

创作者中心发布游戏时，需要勾选发布的是**立即上线**还是**手动上线。**

![CleanShot 2023-09-25 at 11.12.23@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.12.23@2x.webp)

- **立即上线**：审核通过后会显示【上线】状态，表示该版本已经发布到233乐园；
- **手动上线**：审核通过后会显示【等待发布】状态，可以配置版本实验或按照自己想要的时间手动点击上线；

> 若该版本开启了版本实验，则会显示【灰度测试中】，详见[游戏版本实验](https://docs.ark.online/CreatorPortal/VersionManagement.html#版本实验)

![img](https://arkimg.ark.online/1684028257005-126.webp)

### 如何下线游戏

点击目前上线的版本（除版本实验外，一个游戏仅会存在一个上线版本），即可下线游戏；

![CleanShot 2023-09-25 at 11.14.57@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.14.57@2x.webp)

## 版本实验

版本实验是为了方便创作者有效迭代游戏，可通过游戏启动时分发不同的游戏版本，进行版本实验。

版本实验的原理是A/B实验，将线上流量按照设置比例完全随机地分给组A和组B（控制变量），再结合统计方法，得到两组相对效果的准确估计（假设检验）。

实验开启后，线上的用户会完全随机地分给对照组A和实验组B，然后基于用户的行为数据指标进行分析，评估出相比之下更好的版本，帮助创作者进行迭代决策；A/B实验还允许在不同组别之间调配流量，满足游戏低风险迭代的要求。

![CleanShot 2023-09-25 at 11.17.09@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.17.09@2x.webp)

### 开启版本实验

审核通过后，【版本列表】可以看到游戏状态，存在一个【上线】+ 一个【等待发布】，即可开启实验。

![CleanShot 2023-09-25 at 11.20.24@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.20.24@2x.webp)

::: warning 实验过程中，是否可以添加新的版本？

::: 

- 不可以！实验过程中，为了实验准确性，该游戏不可以有新版本上线或下线，会提示你先去关闭实验，关闭实验就可以正常操作游戏下线了；
- 如果这个游戏正在实验中，再发布一个立即上线的版本，审核通过后也不会立即上线，是**等待发布**的状态，需要实验关闭后，再去手动操作上线；

可以通过【游戏服务】-【版本实验】查看当前游戏的实验列表并对实验进行操作，同时也可以在此新增实验。

![CleanShot 2023-09-25 at 11.23.57@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.23.57@2x.webp)

### 设置版本实验

实验信息包含要做实验的游戏、实验名称、实验描述、实验时长等，其中最关键的是选择核心指标。

> 核心指标帮助判定实验是否符合预期的效果，只有能计算置信水平的指标，才能用作核心指标。

![CleanShot 2023-09-25 at 11.37.27@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.37.27@2x.webp)

::: danger **实验过程中的核心指标选什么？**

::: 

建议根据本次实验的目的设置核心指标，鉴于指标本身属性，仅部分指标可以设置为核心指标。例如游戏时长、人均游戏次数以及客户端运行性能相关。

![MetaApp-游戏AB指标选择](https://arkimg.ark.online/MetaApp-%E6%B8%B8%E6%88%8FAB%E6%8C%87%E6%A0%87%E9%80%89%E6%8B%A9.webp)

实验参数主要是设置要进行实验的版本、流量分配，设置完点击调试实验：

- **设置实验版本**：设置本次参与实验的游戏版本，最多可设置2个实验组+一个对照组；
- **流量分配**：可对实验版本进行流量切分（想让小部分用户体验，实验组可设置10%的流量）；
- **添加白名单**：如果想让某些用户进入指定版本，可通过输入233号的方式添加位白名单；

![CleanShot 2023-09-25 at 11.40.10@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.40.10@2x.webp)

调试成功后，会来到实验列表页面，调试中的实验可以去修改实验配置，点击开始即可进行实验。

> 实验开启后，在游戏列表页面，再看自己原来的两个版本，【等待发布】的版本会变成【灰度测试中】

![CleanShot 2023-09-25 at 11.47.16@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.47.16@2x.webp)

### **查看实验报告**

实验报告提供游戏版本迭代依据，可查看实验报告后决策上线哪个游戏版本，实验开始后会在次日早上九点更新数据报告，点击查看报告即可查看实验数据。

![CleanShot 2023-09-25 at 11.49.30@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2011.49.30@2x.webp)

其中包含实验不同版本的用户进组情况占比、版本说明，以及实验设置里核心指标的置信程度。

![img](https://arkimg.ark.online/1684028257005-134.webp)

也可以继续查看实验版本的其他实验指标、新用户&活跃用户的数据情况以及不同进组用户的留存情况。

<video controls src="https://cdn.233xyx.com/online/wTD5FmK86drq1695614338345.mp4"></video>

实验结束后，游戏分发会回到实验开始前，即原来是1.1.89是上线版本，那么结束实验这个版本还会是全量在线，需要根据实验的结果，调整版本的上下线。

![CleanShot 2023-09-25 at 12.01.37@2x](https://arkimg.ark.online/CleanShot%202023-09-25%20at%2012.01.37@2x.webp)

