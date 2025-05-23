
# Breakout 河流生成方案

coenjin(金沛沛)

创建，最后修改于2023-10-17

摘要：这个文字介绍了一个关于河流模拟游戏中的河道的制作过程。通过使用Path组件和Water组件等工具，可以调整河道宽度、高度、Falloff等参数以及添加随机河道形态。在详细Path组件使用参考文档中，可以找到如何输出一张River的mask，并根据这个mask得到其他几张mask。此外，还可以进行高度偏移以挖出河道的深度，以及添加侵蚀的地形细节。最后，可以使用Water组件进行河流模拟，并导出到指定关卡。

# 演示视频

Breakout河流.mp4

# 使用逻辑

*   使用 Path 组件挖出河道，调整河道宽度、高度（Path曲线应该贴紧设定的水面高度）、falloff 等参数，以及 noise 参数以添加随机河道形态；详细Path组件使用参考文档：
    
    Path
    
*   Path会输出一张 River 的 mask，根据这张 mask，可以得到以下几张 mask 用于后续使用：
    
    *   RiverBed，河床，表示整个河流所占区域；
        
    *   RiverSide，河流边缘，即 河床减去River；
        
    *   RiverOuter，河流外的区域，用来给到河流模拟时，避免水流到河流外的区域内；
        
    *   RiverRain，河流降水的区域，用来给到河流模拟时，提供水源；
        
*   基于 River mask，进行高度偏移，挖出河道的深度；基于 RiverSide mask，进行高度偏移，抬高河流边缘地形高度；
    
*   在 RiverSide mask 上添加 real-time erosion 和 ridge，添加侵蚀的地形细节；
    
*   使用 Water 组件进行河流模拟，需要 Custom Rain Masks 中添加 RiverRain mask 作为降水区域，调整降水模式为**相对深度**, 这种模式下可以通过参数来控制河流的深度; 并将 RiverOuter mask 给到 Evap mask、Occlusion mask、Vel Erase mask，以避免河流中的水流到河流以外的区域；另外相对深度模式下, 可以添加平滑后处理, 来防止河流Mesh在边缘翘起.
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-60d5fc197e753fa6398833f563580c43.png)

*   Water 组件会根据水面高度生成一张 mask，可以用这张 mask 来给水面以下区域添加地形权重；
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-dfd5c296e24de0640749e646c8096689.png)

# Bake河流Mesh&Flowmap

Bake河流Mesh, 直接点击下方面板按钮, 即可Bake, Bake河流Mesh过程中, 会输出高质量的河流Mesh, 以及Flowmap

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-43cc3de49ca6bf8d40612df27c52446b.png)

其中河流Mesh的烘培, 可以通过以下参数来控制

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-bd3a8fb9b2792de59b9ddf2f4d89156b.png)

如果对于Flowmap的生成结果不满意, 可以通过添加引导Mesh的方式, 来修正Flowmap的朝向

在Water组件面板中按住 shift+A, 添加引导模型, 每个引导模型, 都有一个**方向**和**影响半径,** 影响半径可以在Water组件中统一调整, 也可以点中单个引导模型, 在 Detail 面板上勾上 override radius, 然后调整 radius 即可.

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-594e620469262bd93cf8ccea89a4bbb0.png)

具体操作如下面视频所示操作即可.

bandicam 2023-10-17 20-21-31-563.mp4

# 切块导出

如果想要把 河流Mesh 切块导出到指定关卡, 可以使用导出工具单独对 河流Mesh Actor 进行切块导出.

1.  新建一个导出预设, 用来专门导出 河流Mesh. 保存到 PCG 关卡文件夹下, 并在导出配置中选择该预设
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-d2608910d983b9741619f24b9041fc0e.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-ee003e1a1370916355587a675cb361b8.png)

2\. 只勾选 Shatter Actor to Tiles, 然后按照下图的方式配置参数

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-1789405154cbc6a26dd5cefbb0856d4d.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-763f8d67c7e652f3309b6be175441dca.png)

3\. 点击运行, 即可导出 河流 Mesh

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-b558507e627c4cc589e2f3b91a5ac49a.png)

# 模拟隧道内的河流

Water 组件模拟河流, 依赖地形数据, 但是有时候需要河流穿过山洞, 这时可以通过临时修改地形高度, 然后模拟完河流之后, 再将原始地形高度设置回去.

首先在用Path挖河床之前, 将原始的地形高度写入到一张高度图中(需要按照下图方式创建, 然后将地形高度(命名为Heightmap)赋值给这张高度图)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-7085f9fc7d2730afe594fa6c78b21bd6.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-2d106b2e038a286951a27df4e818306f.png)

然后通过任意方式修改地形至河床的高度(可以通过Mesh Proejection, 需要通过 modeling tools 或者其他 DCC 软件创建一个代表基准高度的面片, 注意, **该面片需要完整覆盖后面的河床影响范围**)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-59e80ecae2041badc2efe733e0866b32.png)

然后将Path往山洞内延伸, 挖出河床形状, 并通过 Water 组件模拟河流

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-99face5886cb894c175f7148a1c2c718.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-72906364729b21605e92056e5a908ae9.png)

最后计算一张高度混合的Mask(代表山洞处的地形范围), 将一开始保存下来的高度图, 和修改后的高度图做一次混合

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-f62cad9dc460cf73b182230748577244.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-4cd68b5a533f89a2e519e6461d763a01.png)

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

Path

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-48f34ae457aeafc603ab0a403f103594.svg)54
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-0b95f0082c86623bb3ccf41eddc04c8b.png)13
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699332-d56c24c81b5e5f02b023f9382e1ca21d.svg)
