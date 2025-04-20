
# POI LA/LD使用流程

coenjin(金沛沛)

创建，最后修改于2023-06-21

关于 POI 的 capture 和使用说明，请参考这篇文档：[https://iwiki.woa.com/pages/viewpage.action?pageId=4007798134](https://iwiki.woa.com/pages/viewpage.action?pageId=4007798134 "https://iwiki.woa.com/pages/viewpage.action?pageId=4007798134")

# capturedPresetSpawner 组件相关参数

## 基础参数

**起效的实例**：表示对当前场景哪些 poi 起作用，默认是对当前场景中所有 poi 都起作用，不勾选则**对所有实例起效**，就可以手动配置当前 capturedPresetSpawner 组件对哪些 poi 起作用。（这在需要**对不同 poi 区分管理**时会非常有用，比如两个 poi 投射不同的mask)

**Full Sync Levels**：全量更新同步当前场景中的所有 poi ，会从原始 capture 到的 poi 数据中进行同步，如果在当前 PCG 关卡中对 poi 中的内容有修改，会覆盖这些修改。**（因此是一个需要谨慎考虑的操作）**

**Actor 同步模式**：决定当前 capturedPresetSpawner 组件生效的 poi 上，是否生成其中的 actors，如果选 none，则只生成地形和mask，如果选择 Normal，则即生成地形和mask，也会生成上面的 actors;

![](1704699510-acfea8914315d9dd5347bd76b694cc22.png)

点击 poi （点击场景中的 边框 即可以选中 poi），也可以针对场景中单个 poi 个体进行调整

![](1704699510-a46cfc32994ce5c769fc55c65510beac.png)

## Bake Actors

可以将当前 capturedPresetSpawner 组件上起效的 poi 上的 actors bake 到一个指定的关卡上，用于后续美术流程上对这些 actors 进行统一管理。  

![](1704699510-5129065324bfe70259794319b3628cd2.png)

# LD 使用 POI

分两种情况使用:

## 初次放置 POI

需要先在 stack 中**放置 capturedPresetSpawner 组件**，然后将 POI 的资产（Falcon Captured Level Preset）拖入场景中，会在当前场景中生成一个 poi 实例关卡（并非引用原始 poi 数据），实例关卡资产由 capturedPresetSpawner 组件统一管理生成，基本逻辑就是如果已有生成实例关卡，则引用生成的实例关卡，如果没有生成，则会生成一份实例关卡。如果在 PCG 对 poi 有修改，也是保存到实例关卡中，而非保存到原始数据上。在版本管理时，所有实例关卡需要**统一保存上传**；

将 capturedPresetSpawner 上的 **Actor 同步模式改成 Normal**，这样就可以生成 poi 上的 actors；

然后调整高度过渡、权重过渡等与地形的混合效果；

![](1704699510-2b8156e5694e73a677a66ce7edb6ed06.png)

如果对当前 poi 中的 actors 作了修改，想要**保存覆盖回原始 poi 数据**，可以点击 Detail 面板上的 **Sync Actors Back**。

![](1704699510-b8b3998f78638c52e7ad24d8bcae47aa.png)

## 在LA修改后调整 POI

LA会将 poi 中的 actors bake 到一个指定的关卡上进行修改，因此 LA 修改后的 poi 会跟 LD 原始放入的 poi 不同；这时可以先将 capturedPresetSpawner 上的 **Actor 同步模式改成 Normal，**将原始放入的 poi 中的 actors 清空，然后将 LA 在当前 poi 修改后的 actors **拷贝** 一份到 poi 所在关卡上（点中 poi 中的任意一个 actor，即可以看到当前 poi 所在关卡名称），然后就可以对 poi 进行各种操作了（记得把 LA 的 actors 隐藏，否则场景中会有两份 poi）；

另外，如果想要保存 LA 的结果回 LD 的 poi，可以选中 poi，点击 Detail 面板上的 **Sync Actors Back**。

![](1704699510-27e71a236d77d0326bc3d49b307f8497.png)

# LA 使用 POI

因为 LA 会对 POI 做出较大尺度的调整，并且还有对资产进行分类管理的需求，因此可以将 LD 生成的 poi 做一次 bake，**拷贝到**一个指定的美术关卡上，并且**将 Actor 同步模式改成 None**（不生成 poi 上的 actors，只影响地形融合），然后手动将 bake 的 关卡加载到当前 PCG 关卡上。

做上述处理之后，LA 就可以在 bake 关卡上对 poi 上的 actors 做各种操作了。

![](1704699510-60b86a09af5529bcaeeaf2dac641f5cd.png)

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

07-地形Preset(POI)及Ruled Scatter

*   ![](1704699510-48f34ae457aeafc603ab0a403f103594.svg)96
    
*   ![](1704699510-0b95f0082c86623bb3ccf41eddc04c8b.png)22
    
*   ![](1704699510-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](1704699510-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](1704699510-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](1704699510-d56c24c81b5e5f02b023f9382e1ca21d.svg)
