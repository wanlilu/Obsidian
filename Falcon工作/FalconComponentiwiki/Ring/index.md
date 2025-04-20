
# Ring

jcyan(严吉)

创建，最后修改于2023-09-14

![Ring_Banner.png](1704813346-c30a48a493c463c64c467e1c06f47767.png)

> 在UE编辑器中，使用Falcon曲线控制器所形成的内部闭合区域，创建类似高原/盆地/湖泊等地貌形态

# 添加组件

开启Stack面板

*   **Falcon** | **Stack** | **Layer**
    
*   **+Component** | **Landscape** | **Ring**
    

![Stack_Ring.png#400px](1704813346-166841b1de4c0643bfe90a8b5a8470cb)

# 全局控制

*   **Global**
    
    *   **Force Update**：强制刷新
        
    *   **Continous Updating**：持续更新
        

![Ring_Global.png#400px](1704813346-965dbed420b9ce10dfa49979faec8abe)

## 应用场景

*   **Continous Updating**开启时，曲线形态和参数编辑会持续触发地形合成。否则，只在操作确认（例如调整参数后释放鼠标）时更新地形。
    
*   对于小尺寸地图，如曲线数量较少，开启此选项会有更好的操作体验。如曲线数量较多，建议不开启。
    

# 曲线管理

![Ring_RingList.png#400px](1704813346-6e87d79673ef305b91f3a3f6533cb782)

## 列表管理

曲线列表右上角的按钮，分别为：

*   增加
    
*   删除
    
*   复制
    

### 增加/删除曲线

![Ring_PanelOperation_Create_Remove.gif](1704813346-255e1c2b10b01bbea6ffdc6f4bdbb74a.gif)

### 复制曲线

![Ring_PanelOperation_DuplicateRing.gif](1704813346-bcc45f8d7fab228d3fb76233b50e428e.gif)

### 调整层级

![Ring_PanelOperation_ChangeOrder.gif](1704813346-16f86401a2418a53e111fb1132728fd1.gif)

## 列表和视口中曲线的自动关联

> 选择列表或视口中的曲线，与之对应的视口或列表项，会自动产生关联处于选中状态。

### 列表 > 视口

![Ring_PanelLinking_List2Viewport.gif](1704813346-29006af2e8e533e3ff912b34ac6cc729.gif)

### 视口 > 列表

![Ring_PanelLinking_Viewport2List_SingleSelection.gif](1704813346-68a95c2dd915a022757cce8ae0f1c732.gif)

### 聚焦选中曲线

在列表或视口中选择曲线，有两种方式可将当前相机聚焦至所选曲线

*   **选中**曲线 **|** 快捷键 **`F`**
    
*   **双击**曲线列表中的曲线
    

![Ring_PanelOperation_FocusRing.gif](1704813346-d2bd5aaf8b7a64e7f2735494ba6363ea.gif)

# 曲线控制

## 增加控制点

两种方式：

1.  选中曲线线段，**右键 | Insert Control Point**
    
2.  Ctrl + 鼠标左键单击曲线线段
    

![Ring_InsertControlPoint.png](1704813346-a672e566f4e2a93df6f09be8d833753e.png)

## 删除控制点

*   选中曲线控制点，**右键 | Delete Control Point**
    

![Ring_DeleteControlPoint.png](1704813346-b8ba9d47af59c08b403ed9ac7e9a08a5.png)

## 编辑控制点

编辑约束：XY平面（**仅可在XY平面，即水平方向上执行平移/旋转/缩放**）

![Ring_MovePoint.gif](1704813346-fdee83bd0974063055ac8b588387fbda.gif)

当且仅有一个控制点处于选中状态，缩放操作直接应用于切线长度  
![Ring_Visualizer_ScaleSinglePoint.gif](1704813346-05a301f6edc6d2c14f166e5c815cfd91.gif)

## 编辑切线

编辑约束：XY平面

![Ring_MoveTangent.gif](1704813346-8504ff468f495c8e25511e4edfc7e13c.gif)

## 编辑线段

编辑约束：XY平面

![Ring_RotateSegment.gif](1704813346-48172ca354a243423352d9c5b61c04e3.gif)

## 编辑把手

选择箭头状把手，可以整体移动/旋转/缩放曲线

![Ring_MoveKnob.gif](1704813346-2ee83324585986f198bed6a3de09793d.gif)

# 地形控制

![Ring_LandscapeEdit.png#400px](1704813346-9c04a9355aaf9b421d1fc55413ec23a8)

## 全局设置

> 默认情况下，调节此参数，会影响当前组件中的所有曲线

*   **Shape**
    
    *   **Height Falloff**（单位：cm）：高度渐变
        
    *   **Mask Falloff**（单位：cm）：Mask渐变
        
    *   **Inset**：渐变模式，默认关闭表示向外渐变
        

### Inset模式

![Ring_Falloff_Inset.gif#800px](1704813346-00350ae89fd12a5683d400611276aece.gif)

## 曲线独立设置

> 选中曲线后：  
> \- 开启参数之前的选项，可对该曲线属性独立设置，以覆盖全局属性值  
> \- 关闭选项，则使用全局属性值

![Ring_PanelOperation_OverrideFalloff.gif](1704813346-093e53bde89eb6252d4e8cd990322793.gif)

可独立设置参数

*   **Height Falloff**（单位：cm）：高度渐变
    
*   **Mask Falloff**（单位：cm）：Mask渐变
    

## 高度

*   **Elevation**：高度
    

可以选中多条曲线，统一设置高度

![Ring_PanelOperation_OverrideElevation.gif](1704813346-15817253fd194c2371fa0c6b596208b2.gif)

# 自定义输出Mask

> 默认输出Mask名称为 `Ring`，可自行定义名称，实现对各Ring组件使用不同的纹理

开启Path组件面板

*   **Component Mask**
    
    *   **Cusom Mask**：是否启用自定义Mask名称
        
    *   **Custom Mask Name**：自定义Mask名称，可设置为不同于默认值`Ring`的名称
        

![Ring_CustomMask.png#400px](1704813346-b86ac8a3a813a4c0b11e841d84d52d36)

# 高级设置

## 输出内容

开启Ring组件面板

*   **Shape**
    
    *   **Affect Height**：影响地形高度
        
    *   **Affect Weight**：影响地形遮罩
        

![26.png#400px](1704813346-298a113dc275dcd61a8ab4422738e960)

## Falloff平滑度

![Ring_Smooth.png#400px](1704813346-4ee8fb594233f825910b30a22b1f3aa1)

*   设置为最小值，可以形成锐利的边缘过渡
    

![Ring_FalloffBlur.gif#800px](1704813346-40b17c0b5273582f652f4a8e81412af8.gif)

## 曲线全局显隐

必要时可以隐藏当前场景中的曲线，提升编辑器效率

![tapd_20422138_1663673437_27.png#800px](1704813346-8fe8e2b218e174dcc0b808e2677fc51a.png)

优而赞之，手有余香

v\_ryyrlv(吕若怡)，gerayli(李明欣) 点赞

*   本文引用
*   本文被引用

功能盘点

Release-2022.11.18

曲线分布工具 Ring Scatter

*   ![](1704813346-48f34ae457aeafc603ab0a403f103594.svg)197
    
*   ![](1704813346-0b95f0082c86623bb3ccf41eddc04c8b.png)32
    
*   ![](1704813346-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](1704813346-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](1704813346-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](1704813346-d56c24c81b5e5f02b023f9382e1ca21d.svg)
