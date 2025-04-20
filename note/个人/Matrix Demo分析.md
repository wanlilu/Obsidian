## 视频一

#### city layout
![[Pasted image 20231226171039.png]]

三种类型数据 和一些其他的数据
![[Pasted image 20231226171158.png]]

三种道路
![[Pasted image 20231226171522.png]]

用了 模组 拼接
![[Pasted image 20231226171609.png]]
边缘形状为了更好的衔接， 有一定的起伏

![[Pasted image 20231226171758.png]]

#### freeway 高速公路部分

![[Pasted image 20231226172217.png]]

帮助视频 创作者规划路线
![[Pasted image 20231226172305.png]]

![[Pasted image 20231226172452.png]]

![[Pasted image 20231226172504.png]]

#### Lots and building volumes

![[Pasted image 20231226172641.png]]
![[Pasted image 20231226172703.png]]

![[Pasted image 20231226172757.png]]

![[Pasted image 20231226172815.png]]

![[Pasted image 20231226172839.png]]

![[Pasted image 20231226172922.png]]

![[Pasted image 20231226173048.png]]

![[Pasted image 20231226173103.png]]

#### The city ground 

  ![[Pasted image 20231226173214.png]]
  ![[Pasted image 20231226173305.png]]
  ![[Pasted image 20231226173323.png]]
  ![[Pasted image 20231226173354.png]]

![[Pasted image 20231226173506.png]]

![[Pasted image 20231226173624.png]]

![[Pasted image 20231226173641.png]]
将没有描述的用地 再按类别分为：广场 ， 高速 ，停车，分别处理
![[Pasted image 20231226173711.png]]

预先做好 资产包blueprint ，scatter上去 
![[Pasted image 20231226173917.png]]

![[Pasted image 20231226173943.png]]

![[Pasted image 20231226174040.png]]

![[Pasted image 20231226174151.png]]

![[Pasted image 20231226174209.png]]

![[Pasted image 20231226174234.png]]
![[Pasted image 20231226174339.png]]

#### city audio
![[Pasted image 20231226174439.png]]

#### building generate
![[Pasted image 20231227100412.png]]

![[Pasted image 20231227100434.png]]
没栋建筑都有tag，并且颜色是他的style
![[Pasted image 20231227100616.png]]

![[Pasted image 20231227101334.png]]

![[Pasted image 20231227101825.png]]
赋予 bdf的类型，并通过类型计算出 楼层间距，获取切片
![[Pasted image 20231227101842.png]]
通过点积获取拐点类型，并通过顺序获得序号  ，同时考虑 是否两个面要同时考虑，还有有优先级排序

![[Pasted image 20231227102038.png]]

![[Pasted image 20231227102510.png]]

![[Pasted image 20231227103302.png]]
![[Pasted image 20231227103310.png]]

![[Pasted image 20231227103320.png]]

![[Pasted image 20231227103622.png]]

![[Pasted image 20231227104312.png]]

![[Pasted image 20231227104439.png]]