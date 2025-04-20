
# Path

jcyan(严吉)

创建，最后修改于2022-10-24

# Path

![38.png#1000px](1704813318-de01146e71f4893df081008087d3d960.png)

> 在UE编辑器中，使用Falcon曲线控制器修改地形，可创建类似道路/峡谷等地貌形态

# 添加组件

开启Stack面板

*   **Falcon** | **Stack** | **Layer**
    

![01.png#1000px](1704813318-a133a37951b1b8c1fb15397df16593f6.png)

*   **+Component** | **Landscape** | **Path**
    

![00.png#400px](1704813318-37a201a06a017ae03ca6c101ae468473)

# 全局控制

*   **Global**
    
    *   **Bypass**：关闭合成
        
    *   **Force Update**：强制刷新
        

## 应用场景

*   场景较大（如>4k），组件合成时间过长时，考虑开启 **Bypass**
    
*   此时修改曲线时，比如移动控制点等，组件不会自动触发合成
    
    *   必要时点击 **Force Update** 手动强制刷新，操作流程类似于离线烘焙
        
*   曲线修改完毕后，可以关闭 **Bypass**，返回自动刷新状态
    

![1.png#400px](1704813318-3f13f946b456afdf21d5e02d709b6f48)

# 曲线控制

## 起始控制点

添加组件后，保持组件的选中状态，在地形上任意位置单击鼠标左键，即可创建曲线的起始控制点  
![02.png#1000px](1704813318-68956c8a373bfb2791a2d893926b442e.png)

## 增加控制点

Ctrl + 鼠标左键单击，在地形中添加曲线控制点  
![03.png#1000px](1704813318-57b1006c0adf2a1ce6dee9b741d66f13.png)

## 曲线显示设置

开启Manipulator面板

*   **Falcon** | **Manipulator**
    

![04.png#1000px](1704813318-cb84e1969c0738d417599eab796bd05a.png)

选中曲线的任意控制点或线段，可设置：

*   曲线控制点尺寸
    
*   曲线线段尺寸
    
*   颜色
    

# 地形控制

## 高度

选中控制点或者线段，通过改变垂直方向的高度，实现对地形高度的修改  
![05.png#1000px](1704813318-ca09dafde7f6a28d8d63d0c97890bcc6.png)

下压  
![06.png#1000px](1704813318-84eb5d40d98878804fa4580322ca9465.png)

上抬  
![07.png#1000px](1704813318-c3a481e0595fe818604e0b7f762fdde4.png)

## 宽度/侧向渐变

![36.png#400px](1704813318-570387ebeb0274977e51d978f395ad64)

### 全局设置

> 设置所有控制点的属性，即影响曲线的全局表现

开启Path组件面板

*   **Point Attribute**
    
    *   **Width（单位：厘米）**：宽度
        
    *   **Side Falloff（单位：厘米）**：侧向渐变
        

![08.png#400px](1704813318-04de1fc412106a9cd88a8fac47d203fe)

调整参数后，点击`Set All`按钮，即可确认修改

#### 示例

*   **Width**: 800
    
*   **Side Falloff**: 1200
    

![09.png#1000px](1704813318-af9bee99d32c3e5ae46cc229511ff718.png)

### 独立设置

> 独立设置某单个控制点的属性

开启Path组件面板

*   **Point Attribute**
    

点击对应属性名称左边的圆点，开启控制点属性属性操纵杆  
![10.png#400px](1704813318-c44335aab508b6b1fc56519733c10497)

曲线控制点会显示箭头形状的操纵杆  
![11.png#1000px](1704813318-3f9bbbe5ae552031e5f61a3245a635a1.png)

选中属性操纵杆，拖动位置以设置路边宽度或侧向渐变  
![12.png#1000px](1704813318-7e2569e979dfdddbec630c6778257ef6.png)

设置完毕后，点击属性名称左边的圆点，可以隐藏属性操纵杆

## 纵向渐变

> 曲线首尾两端控制点的纵向渐变

开启Path组件面板

*   **Shape**
    
    *   **End Falloff（单位：厘米）**：纵向渐变
        

![26.png#400px](1704813318-f11589f13c27232a7dadea88eff96b5e)

### 示例

*   **End Falloff**：500
    

![27.png#1000px](1704813318-481c7268da037f0f700f93347ab03ed9)

*   **End Falloff**：2000
    

![28.png#1000px](1704813318-83ecfe1d0ddd4ce51ed230e3c93c627e.png)

## 侧向渐变曲线

> 使用曲线控制侧向渐变的形态

开启Path组件面板

*   **Shape** | **高级设置** | **Side Falloff Curve**
    

![29.png#400px](1704813318-5567d353397319a1deb893ba38a54080)

鼠标左键双击曲线面板，可以开启独立的曲线编辑窗口  
![34.png#400px](1704813318-5d794abab4dda2adf5fc0697b5776f13.png)

曲线编辑窗口操作：  
![35.png#400px](1704813318-34bc63774f7732eead1a0020d677bbb7.png)

*   Shift + 鼠标左键单击：添加Key
    
*   选中Key | 鼠标右键单击：菜单可以设置插值方式
    
*   选中Key后，可以编辑切线
    

### 示例

![30.png#1000px](1704813318-80ff3a0bd069a699e06b99dcd18b8939.png)

![31.png#1000px](1704813318-133cde6cc57ba8d44d58262221ae84a3.png)

![32.png#1000px](1704813318-c4397089466f248e983829bedbd08d1c.png)

![33.png#1000px](1704813318-00b9674250f3179f2bacaab70d3f1929.png)

# 遮罩（Mask）控制

> 输出Mask，用于Weigthmap组件，对路面部分设置材质

## 设置Weightmap组件

添加或设置已有Weightmap组件

*   指定材质右边的下拉菜单
    
    *   **Add Parameter** | **Add PathFilter as Custom**
        

![13.png#400px](1704813318-fd25e4d47ee62120a49f5b5e74789dcf.png)

设置权重范围  
![14.png#400px](1704813318-5586524ade9b0fdd0d6bbdae217d0238)

示例  
![15.png#1000px](1704813318-90ef5daee4ee6bb5fff097d334cace1f.png)

## 遮罩宽度

> 默认状态，Mask的宽度与路面宽度一致，需要时可以设置两者之间的差值

开启Path组件面板

*   **Shape**
    
    *   **Mask Offset（单位：厘米）**：遮罩宽度
        

![16.png#400px](1704813318-ac56f2c90ee3fcd55ff58587a9d4fc59)

正值表示外扩

*   **Mask Offset**：400
    

![17.png#1000px](1704813318-187f500f0bf129eccb2806dec3c8a8e9.png)

负值表示内缩

*   **Mask Offset**：-400
    

![18.png#1000px](1704813318-4006c32164a53b9a97e041b8b9976227.png)

## 遮罩渐变

开启Path组件面板

*   **Shape**
    
    *   **Mask Falloff（单位：厘米）**：遮罩渐变
        

![19.png#400px](1704813318-353047e00f1ef306297fcfbe78a6daf8)

### 示例

结合宽度和渐变控制Mask的形态

*   **Mask Offset**：-400
    
*   **Mask Falloff**：2000
    

![20.png#1000px](1704813318-f3f176358721e10fece0c38ed5d94872.png)

# 边缘噪声（Noise）

> 对曲线边缘的高度或Mask增加Noise控制，形成随机扰动的效果

## 整体设置

> 全局Noise同时应用于高度和Mask

### Noise参数

开启Path组件面板

*   **Noise**
    
    *   **Global Noise Factor（单位：厘米）**：强度
        
    *   **Global Noise Scale**：频率
        
    *   **Global Noise Shift（单位：厘米）**：偏移
        

![21.png#400px](1704813318-d6edd53a1244b6f0ffad4c527e5b2f8a)

#### 示例

*   **Global Noise Factor**：600
    
*   **Global Noise Scale**：2
    
*   **Global Noise Shift**：10
    

![22.png#1000px](1704813318-0d2424c6ca907f27cb843fe15bd84c42.png)

## 遮罩独立设置

> 遮罩专有Noise，可以独立设置，亦可叠加至全局Noise

### Noise参数

开启Path组件面板

*   **Noise**
    
    *   **Mask Noise Factor（单位：厘米）**：强度
        
    *   **Mask Noise Scale**：频率
        
    *   **Mask Noise Shift（单位：厘米）**：偏移
        

![23.png#400px](1704813318-fd7a9d71399c7bbede4b345997f70c40)

#### 示例

*   **Mask Noise Factor**：300
    
*   **Mask Noise Scale**：20
    
*   **Mask Noise Shift**：0
    
*   **Global Noise Weight**：1
    

![24.png#1000px](1704813318-824f6f39df6ca38f9725d2e81003d7ad.png)  

### 全局Noise的权重

> 控制Mask是否收全局noise影响

*   **Noise**
    
    *   **Global Noise Weight**：全局Noise的权重
        

#### 示例

若权重为0，表示遮罩不受全局noise的影响，以下图为例，路面边缘存在扰动，而遮罩为平滑曲线  
![25.png#1000px](1704813318-d3e586371496f89730fa832865722389.png)

# 自定义输出Mask

> 默认输出Mask名称为 `PathFilter`，可自行定义名称，实现对各Path组件使用不同的纹理

开启Path组件面板

*   **Component Mask**
    
    *   **Cusom Mask**：是否启用自定义Mask名称
        
    *   **Custom Mask Name**：自定义Mask名称，可设置为不同于默认值`PathFilter`的名称
        

![CustomMask.png#400px](1704813318-edc7a1c100cafe7e00187fd3e10d1974)

## 演示

[如何为Path生成独立的Mask？](https://iwiki.woa.com/pages/viewpage.action?pageId=1628326023)

# 输出水体模拟的降雨区域

> 在曲线起点位置，输出名为 `RainSpot`的圆形mask，可作为水体模拟的降雨区域

开启Path组件面板

*   **Component Mask**
    
    *   **Rain Spot Mask**：是否输出降雨区域
        
    *   **Rain Spot Radius**：原型Mask的半径
        

![RainSpot.png#400px](1704813318-61e3bb1e4f4404648b65eac10da3f75d)

## 演示

ExportRainSpotMask.mp4#1000px

# 高级设置

## 输出内容

开启Path组件面板

*   **Shape**
    
    *   **Affect Height**：影响地形高度
        
    *   **Affect Weight**：影响地形遮罩
        

![26.png#400px](1704813318-f11589f13c27232a7dadea88eff96b5e)

## 实时更新

开启Path组件面板

*   **Shape**
    
    *   **Realtime Update**：是否实时更新
        

![27.png#400px](1704813318-481c7268da037f0f700f93347ab03ed9)

如取消勾选，修改曲线（包括添加/删除/移动控制点等）的操作，不会对地形产生影响。**对于大尺寸地图，如果修改曲线的耗时较长，可以考虑先关闭实时更新，曲线基本设计完毕后，再开启实时更新进行微调**。

## 组件面板与曲线关联

开启Stack面板

*   点击组件面板中的Path组件，视口中会选中对应曲线的最后一个控制点
    
*   点击视口中的曲线，组件面板会选中对应Path组件，并切换至对应的Layer
    

![28.png#1000px](1704813318-83ecfe1d0ddd4ce51ed230e3c93c627e.png)

注意：**双向关联的特性只有在当前为Stack面板时方可生效**，即上图方框中的按钮

## 曲线全局显隐

必要时可以隐藏当前场景中的曲线，提升编辑器效率

![tapd_20422138_1663673437_27.png#1000px](1704813318-3be6be8c337c9caf41e9d6f0e8c32f0f.png)

# 实验特性

> 一些不影响当前功能，尚处于实验状态的特性，后续可能会进行修改或移除

## 去除褶皱

> 曲线如存在曲率过大的点，同时该点的地形高度与周边差距较大时，该点的地形高度会形成褶皱。

开启Path组件面板

*   **Intersection**
    
    *   **Fix Self Intersection**：是否去除褶皱
        

![29.png#400px](1704813318-5567d353397319a1deb893ba38a54080)

注意：

*   **开启该功能，会增加一定程度的计算量，同时如果曲线存在自相交，则交叉位置的高度数据可能会出现不可预见的结果。**
    
*   **前文中提到，褶皱只会在曲率过大同时存在较大高度差时出现。即便存在褶皱，叠加其他地形修改组件或材质表现，也可以消除视觉上的影响。可以切换设否去除，对比前后的视觉表现，仅在确有必要时开启该功能。**
    

### 示例

![30.jpg#1000px](1704813318-ef19d7c0d9682d989e039dd6d0891fce)  
![31.jpg#1000px](1704813318-0f36c490223774ffcb3a05b805a4ce02)

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

如何为Path生成独立的Mask？

*   ![](1704813318-48f34ae457aeafc603ab0a403f103594.svg)639
    
*   ![](1704813318-0b95f0082c86623bb3ccf41eddc04c8b.png)64
    
*   ![](1704813318-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](1704813318-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](1704813318-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](1704813318-d56c24c81b5e5f02b023f9382e1ca21d.svg)
