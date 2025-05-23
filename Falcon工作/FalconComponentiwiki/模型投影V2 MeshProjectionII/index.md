
# 模型投影V2 MeshProjectionII

machzhang(张诚)

创建，最后修改于2023-11-01

捕获指定 Actor 的高度，并输出到高度图或任意 Stack Mask 中

# Quick Start

### Mesh 影响地形

*   新建 MeshProjectionII 组件
    
*   场景中拖入 Mesh 等 Actor 作为高度输入
    
*   从大纲中拖动 Actor 到“投影目标”下的绿框  
    
    ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-6f476252c0161adc15c0f202ac0953a0.png)
    
*   点击 Project Mesh， 地形就会隆起到 Actor 的高度。点击 Toggle Material 使用半透明材质  
    
    ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-83ad693263d5033a8b73f014ea97c7c5.png)
    
*   如果出现下面的提示，点击 Move Targets，把 Actor 和组件放到同一张子地图中保存  
    
    ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-ac3b7ff9184aae69e4681a2fac8d96f4.png)
    

### Mesh 影响 Mask

*   照前一节操作
    
*   取消“输出高度”
    
*   选中“输出 Mask”,并填入 Mask 名  
    
    ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-57184b61622d2f26f2d0bf82a35ec98b.png)
    
      
    
    ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-abb3636c55631caa0d964b2de340489a.png)
    

# 参数与动作

### Targets

*   **Project Mesh**：更新烘焙高度并刷新 Stack 结果
    
    *   需要手动点击更新
        
*   **Move Targets**：将目标 Actor 移动到组件所在的子关卡内
    
*   **Select All**：选择所有目标 Actor
    
*   **Toggle Material**：切换半透明材质
    
*   **Toggle Visibility**：切换显示隐藏  
      
    
*   投影目标：配置目标Actor（要投影的 Actor 列表）详见
    
    Actor Pred
    
*   只显示投影目标：选中当前组件时，世界大纲只显示投影目标
    
*   输出：在 Stack 上的输出，可以有任意多个，都基于当前的烘焙结果
    
    *   合成方式：烘焙高度与前面 Stack 输入高度之间合成方法
        
        *   替换：某处如果有烘焙高度结果，就使用烘焙高度，否则使用输入高度
            
        *   最大，最小：烘焙高度和输入高度两者取最大最小值
            
        *   单独：某处如果有烘焙高度结果，就使用烘焙高度，否则输出0
            
        *   二值：某处如果有烘焙高度结果，就输出1，否则输出0
            
    *   模糊半径：将输出高度进行平滑处理
        
    *   输出高度：将高度结果输出到 Stack 的 高度图
        
    *   输出Mask：按下面的规则输出到 Stack Mask
        
        *   过渡阈值=0：某处如果有烘焙高度结果，就输出1，否则输出0
            
        *   过渡阈值>0：输出 _高度图变化大小/过渡阈值_
            
    *   输出 16 Bit Mask：将高度结果输出到指定的16位Mask中
        

### Modifiers

*   投影方向：烘焙使用Actor的上面高度还是下面高度
    
*   平移： 烘焙时平移Actor
    
*   扩展/收缩大小：将烘焙结果进行均匀的扩展/收缩
    

### Configs

*   外部模式：为了避免 Actor 和烘焙结果两者中只有一边保存出现的困惑，默认会强制所有目标Actor保存在组件所在的之地图中。选中这个选项可以去掉这个限制
    
*   参数修改后重新烘焙，目标编辑后重新烘焙：是否要自动在编辑后烘焙并更新
    

# 计算方式

大体的计算流程：

*   点击 Bake Actor 时
    
    *   使用目标 Actor，从上往下或从下往上地捕获高度
        
    *   进行平移和拓展搜索，生成烘焙高度
        
    *   生成的烘焙高度会在保存组件地图时被存下
        
*   每次 Stack 更新时
    
    *   使用指定的合成方式，将输入高度和烘焙高度进行合成，生成输出高度
        
    *   比较输入高度和输出高度，生成输出 Mask
        
*   完整的计算流程图
    
    *   图中斜体代表参数
        

**投影  
**_投影方向_

投影 投影方向

**修改**  
拓展/收缩

修改 拓展/收缩

**  
Actor**  
_投影目标_

Actor...

**  
其他组件的烘焙结果**  
_引用外部烘焙结果_  

其他组件的烘焙结果引用外部烘焙结果...

**  
烘焙结果**  

烘焙结果...    **Bake 时**    Bake 时

**合成时**

合成时

**  
Stack  
输入高度**

Stack...

**合成高度**  
_平移  
模糊  
合成方式  
_

合成高度平移模糊合成方式...

  
**Stack**  
**输出高度**  
_输出高度_

Stack...

  
**  
Stack**  
**输出16bit Mask**  
_输出16bit Mask  
16bit Mask 名  
_

Stack...

**合成Mask**  
_Mask过渡阈值  
_

合成Mask...

  
**  
Stack**  
**输出Mask**  
_输出Mask  
Mask名  
_

Stack...

**点状投影**_  
点状投影  
点状投影大小_

点状投影点状投影点状投影大小...

Text is not SVG - cannot display

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

Actor Pred

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-48f34ae457aeafc603ab0a403f103594.svg)92
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-0b95f0082c86623bb3ccf41eddc04c8b.png)19
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813437-d56c24c81b5e5f02b023f9382e1ca21d.svg)
