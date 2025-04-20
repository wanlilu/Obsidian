## 交互

#### pcg输出与layer 关联
![Pasted image 20240123095640](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123095640.png)
当关闭layer显示时，对对应的pcg output关闭显示

#### 基于项目隐藏工具
![Pasted image 20240123100429](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123100429.png)


#### 引擎内控制器
因pcg节点分 引擎内计算节点和houdini计算节点。
完善引擎内的控制器，有助于（1）应用于引擎内计算的交互 （2）加速Houdini计算节点的输入 (3)带来更好的交互体验
![Pasted image 20240123112628](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123112628.png)
![Ring_PanelOperation_Create_Remove.gif](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813346-255e1c2b10b01bbea6ffdc6f4bdbb74a.gif)
![Ring_RotateSegment.gif](https://raw.githubusercontent.com/wanlilu/imgBed/main/note1704813346-48172ca354a243423352d9c5b61c04e3.gif)

offline 改进
1）节点小不好控制，ue原生曲线交互设计不好
![Pasted image 20240123114251](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123114251.png)
2）通过添加路口，原来曲线会被分割成两条曲线，这样比较符合大多数pcg控制逻辑。
如当添加完交叉路口后，之前的曲线会被两个不同街区所拥有
![Pasted image 20240123114838](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123114838.png)

#### 节点面板
##### 是否有必要添加，输入输出已确定
![Pasted image 20240123121743](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123121743.png)

##### 添加搜索功能
![Pasted image 20240123141851](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123141851.png)
![Pasted image 20240123141820](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123141820.png)
##### 待更新提示
上层当usd节点不会自动更新，这是应在面板有提示
![Pasted image 20240123153800](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123153800.png)
#### mask 操作
![Pasted image 20240123121326](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123121326.png)
每个操作需要添加一个节点，

#### 主界面
##### 主界面工具控制器混乱
![Pasted image 20240123171443](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123171443.png)
![Pasted image 20240123171521](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123171521.png)
需要添加类似 content filter 的功能


#### 辅助显示
1. high line
	![Pasted image 20240123111706](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123111706.png)

## bug
##### 只对第二个节点输入输入改动，两个计算节点全变cook状态
![Pasted image 20240123144757](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123144757.png)

##### cook create node 之前要先cook input 不然卡顿


## 不清楚的问题
#### 如何给pcg 工具添加mask 属性输入


## PCG框架
#### 开启关卡后不能自动从下至上mix，导致视口显示效果不对
![Pasted image 20240123170802](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123170802.png)

#### 需要重新计算才能正确显示吗？ 
重启关卡后开关每个层都有问题

![Pasted image 20240123171148](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123171148.png)

#### mix卡顿，需要进一步了解offline的合成方法
可以添加falcon 类似的 global cache、合成异步功能

#### 重启后关闭上层节点会导致
建议从框架上，在每个sublayer里添加cache
![Pasted image 20240123172910](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123172910.png)


## 功能意见
##### Mask Noise节点
noise 可以调节height ，也可以用来生成mask。
height 范围 landscape min 到 max，mask 范围0 - 1
生成的mask 再使用 mask operation叠加，输出更过mask变体，用来撒点制备等
![Pasted image 20240123140852](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123140852.png)

##### 地形塑造 节点过少
- [x] 录制基础地形节点 📅 2024-01-24
	地形塑造一般分为 三个基础不知：地势搭建，noise、terrace地形起伏，filter地形细节塑造，地形侵蚀，filter地形修复

##### Mask 与Weightmap 搭配非常不直观且繁琐


##### 没必要一直在log里打印 内存占用

![Pasted image 20240130115617](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240130115617.png)


## 崩溃信息
##### stamp node
![Pasted image 20240123120822](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123120822.png)
##### 编辑曲线回退崩溃
![Pasted image 20240123150412](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123150412.png)
##### 本地cook无反应，然后崩溃
![Pasted image 20240123181004](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240123181004.png)

## 优势
1. 稳定，多数节点转移至houdini里计算，即便出问题也不会导致unreal崩溃
2. 