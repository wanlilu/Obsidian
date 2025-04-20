
# 音频建筑体积生成

allenpzhang(張鵬)

创建，最后修改于01-03 16:24

## 工具作用：

2023/12/18 新加功能：现在默认会针对ACTOR生成convex外壳的结果，适用于普通物件，例如围墙，汽车，草堆等。

如钩选工具窗口内的“一起计算”也可以让相邻物体生成一个统一的MESH，例如一排围墙，只生成一个BOX。

此工具旨在针对类似建筑的ACTOR生成一个概括其体积的static mesh资产，并摆放到原actor所在的位置，然后音频流程中可以使用此物体在碰撞事件中进行触发。

由于此工具在不需要任何上游信息的情况，完全依赖模型进行生成，所以不能保证结果的正确性，需要使用者在生成之后，对结果进行判断、编辑等，进行最终确认。

## 使用流程：

视频演示：

针对一般物件生成外壳或者box的MESH，然后通过菜单，执行生成蓝图实例的操作（将蓝图实例化到场景，并设置其mesh为工具生成的mesh）.

bandicam 2023-12-18 21-44-20-419.mp4

重点设定，如下图：

“一起计算”：不同物体共同生成结果，如果物体较近，则会生成连续的一个外壳。

对于特殊物件，不想和其他物体一起计算的，比如很近，但是想分开算，建议分多次进行计算。

资产类型：默认简易资产，即生成外壳的简易物体，如围墙。建筑资产是旧的形式，主要针对建筑生成类似房间的MESH。

BOX阈值：如果一个物体的外壳和BOX比较接近，则会变成一个BOX，这个BOX未来可以用标准的volume音频体积进行替换。

强制生成BOUND：不生成convex外壳，强制生成BOX。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-b035792391d9a13bf68c91cdf2e89f16.png)

生成MESH后，选中它们，执行菜单Falcon->Falcon audio volume placement，即可生成蓝图实例。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-096a9acd59e9db06eb4ea8e63143d432.png)

上面PYTHON执行的本质是将蓝图模板实例放入场景，

如果报错，说找不到蓝图模板，可以将下面这个蓝图模板放到如下路径，如果路径没有，新建名字相同的文件夹层级即可

CA和TK: Inbattle/blueprints/Sound/AudioVolume  
NRC: Game/NewRoco/Modules/Audio/AudioVolume  
NB: /Game/Debug/Audio/AudioVolume

然后将名为BP\_auidoVolume\_mesh.uasset的蓝图放在该路径下。

BP\_auidoVolume\_mesh.uasset

136.3KB

（旧视频：针对建筑类）请看视频：

audioVolumeGenerating.mp4

由于生成还是需要一定时间的（每个生成大概耗时1-3分钟），所以建议对需要的 ACTOR 进行批量生成，然后一起查看效果，筛选结果。

批量生成过程中，可以点击run in background,使任务后台运行，然后可以进行其他操作。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-089aa169a4387cc005973562a6ecac91.png)

## 创建方法：

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-a928c6dd7effed8f8e6d1880aaa36918.png)

1.选择编辑器模式：Falcon modes

2.左侧找到相应位置

3.**如果找不到Audio Volume Generating,可以尝试点击下方的update remote params panel**

然后打开批处理窗口：**之后如果仍然想要打开此文档，查看使用方法，可以点击工具右侧的问号按钮。**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-ba5d7eadb9bd4ccb4d3eea1b9c383875.png)

## 参数含义：

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-7ca79c3835cdb30efe38e89c501f547f.png)

左侧为要进行批量计算的ACTOR，可以进行选择之后，批量拖入。

右侧参数含义：

所有音频体积放入总文件夹：是否将不同ACTOR生成的资产放入一个总的文件夹.

总文件夹名称：当所有生成的内容放置于一个总文件夹时，可以在此设置总文件夹的名称。

文件夹后缀：在大纲中static mesh上级目录的后缀

名字前缀：生成的staticMesh资产的名字前缀

选择LOD级别：是否选择某个LOD级别进行计算。若没有LOD，则默认关闭即可。

使用LOD级别：当资产有多个LOD的时候，选择使用哪个LOD级别进行计算

排除碰撞体的计算：默认开启：设定在进行计算的时候，是否排除考虑碰撞体

资产保存路径：设定生成的staticMesh资产保存的路径

优而赞之，手有余香

成为第一个点赞的人

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-48f34ae457aeafc603ab0a403f103594.svg)73
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-0b95f0082c86623bb3ccf41eddc04c8b.png)12
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704816853-d56c24c81b5e5f02b023f9382e1ca21d.svg)
