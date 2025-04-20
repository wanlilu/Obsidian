
# 地形塑造Filters

reachen(陈主润)

创建，最后修改于2023-02-17

效果预览/示例流程：

FilterTurbulence2.0HD.mp4

#### 部分通用参数说明

**Roughness：** 后处理平滑程度，默认0 （大于0时将启用，但对性能有轻微影响，建议<=3）  
**Iterations：** 迭代次数，迭代次数越高，细节程度越高；最大为10  
**Levels：** 控制每个**Iteration**的强度；共10级，但不同组件可生效的最小级数不同；值范围\[0, 1\]  
**Turbulence：** 细节扰动程度，0为不扰动  
**Seed：** **Turbulence**的随机扰动种子

* * *

### Canyon 峡谷

* * *

### Cliff 悬崖

**Slope：** 生效坡度范围，0~90度

* * *

### Contrast 对比减弱

* * *

### Deflate 收缩

* * *

### Fills 全局填平

* * *

### FillSlope 坡度平滑/填平

**Slope：** 生效坡度范围，0~90度

* * *

### Inflate 膨胀

* * *

### Ridge 山脊生成

**Slope：** 生效坡度范围，0~90度

* * *

### Rocky 岩化山体生成

**Slope：** 生效坡度范围，0~90度

* * *

### Valley 山谷生成

**Slope：** 生效坡度范围，0~90度

* * *

\*\*法线问题：\*\*

替换引擎路径下的 \\Engine\\Shaders\\Private\\LandscapeLayersPS.usf：

LandscapeLayersPS.usf

NaNPB

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

功能盘点

2022-11-21 reachen

*   ![](1704813846-48f34ae457aeafc603ab0a403f103594.svg)114
    
*   ![](1704813846-0b95f0082c86623bb3ccf41eddc04c8b.png)23
    
*   ![](1704813846-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](1704813846-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](1704813846-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](1704813846-d56c24c81b5e5f02b023f9382e1ca21d.svg)
