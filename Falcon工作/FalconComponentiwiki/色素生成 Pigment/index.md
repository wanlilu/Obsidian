
# 色素生成 Pigment

machzhang(张诚)

创建，最后修改于2023-12-12

_将 Mask 与权重合成到一张彩色纹理上_  
  
  

_**渐变 Mask**_

渐变 Mask

LUT Ramp

LUT Ramp

上一层 Pigment

上一层 Pigment

清空颜色?  
_**Clear Color**  
_

清空颜色?...

_**LUT  
Atlas 纹理**_

LUT...

选择 LUT Atlas 横向行号  
_**渐变 LUT 纹理行号**_

选择 LUT Atlas 横向行号...

选择 LUT Atlas 纵向范围  
_**渐变 LUT 列范围**_

选择 LUT Atlas 纵向范围...

Ramp 合成

Ramp 合成

_**Color Grade  
对比度 饱和度 Gamma ...**  
_

Color Grade...

_**透明度 Mask**_

透明度 Mask

层输出 Pigment

层输出 Pigment

颜色混合  
_**混合模式**_

颜色混合 混合模式

透明度混合

透明度混合

生成渐变颜色（可选）

生成渐变颜色（可选）

**_Pigment  
RT_**

Pigment...

上一层  
 Pigment

上一层  Pigment

_**基础颜色纹理**_

基础颜色纹理

**_基础颜色  
来源_**

基础颜色 来源

颜色混合  
（相乘）  

颜色混合（相乘）...

# 图例

**_粗体_**：选项.

图例 粗体：选项.

生成基础颜色（可选）

生成基础颜色（可选）

_**色调**_

色调

每一层

每一层

上一组件 Pigment

上一组件 Pigment

透明度混合

透明度混合

_**Region Mask**_

Region Mask

层输出 Pigment

层输出 Pigment

本组件输出 Pigment

本组件输出 Pigment

**_Update MFScenery_**

Update MFScen...

Text is not SVG - cannot display

根据 Pigment Layers 中的配置，按从上向下的顺序，以 Alpha Mask 作为蒙版，向纹理中混合由 LUTMask 生成的颜色。

注意结果的范围取决于组件所在层的范围，默认大小是 Stack 层的尺寸。

# 计算方法

以 BaseColor 或上一层输出作为基础，对每一层

1.  使用 LUT 配置，将单通道的 LUT Mask 映射为彩色的 LUT 结果 
    
2.  使用 Blend Mode 指定的混合方法, Alpha Mask 指定的透明度混合**上一层输出**与 LUT 结果 
    

# Pigment 层配置

*   **Enable**：是否启用 
    
*   **LUT**：LUT 设置
    
*   **LUT Mask**：被 LUT 映射为颜色的 Mask
    
*   **Alpha Mask**：不透明度 Mask。置空时默认为完全不透明
    
*   **Blend Mode**：与上一层的混合方式。参照 Photoshop 混合模式
    
    *   _A代表基层，B代表混合层，C代表结果色_
        
    *   ![](1704813509-29e130efc01a11757a9a1504040d2fc8.png)
        

# 其他配置和功能

*   **Base Color**：作为混合基础的颜色
    
*   **Pigment RT**：混合目标的渲染目标，可以自行指定或者让组件自动生成
    
*   **Offset**：全局偏移，以地形坐标为单位
    
*   **Export as Image**：将结果保存为图片文件
    

Draw.io

画图工具，支持流程图、泳道图等图表

Pigment1.mp4

FalconTestBed \[DebugGame\] - Unreal Editor 2022-04-29 17-15-33.mp4

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

功能盘点

*   ![](1704813509-48f34ae457aeafc603ab0a403f103594.svg)388
    
*   ![](1704813509-0b95f0082c86623bb3ccf41eddc04c8b.png)52
    
*   ![](1704813509-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](1704813509-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](1704813509-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](1704813509-d56c24c81b5e5f02b023f9382e1ca21d.svg)
