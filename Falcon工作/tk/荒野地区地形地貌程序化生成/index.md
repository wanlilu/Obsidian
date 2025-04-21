
# Breakout 荒野地区地形地貌程序化生成

coenjin(金沛沛)

创建，最后修改于2023-09-27

包含 **南部地区** 和 **核心区域** 的地形地貌程序化生成

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-b3cc08e22a69047f11f24454e6af0f50.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-cbeea57a853956dd73261f7897f553db.png)

荒野地区地形地貌，根据 LD 摆放的不同类型白盒作为引导进行生成（白盒输入规范参考下文）。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-c73b597f8c4ddc5571b187a4935ebe22.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-4ecccccd50487d32eda34dbfb5a23457.png)

# 组件逻辑

与荒野区域地形地貌相关的有两个层，分别是 Land\_Plan 和 Land\_South/Land\_Core；

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-ae6de90a57035a9a630c0b1d4392762c.png)

## Land\_Plan

用来确定荒野区域的合成范围，给到 Land\_South/Land\_Core 层作为 region mask

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-d938e0c85d324b958a2b5c934ca0d27d.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-ba8e09c3493254c22101f8fdd1dea3f1.png)

这个区域目前是通过手绘的方式来确定，进入 variant mask 的 Component mask 中即可看到对应的手绘mask

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-e857bb51cf5e43fecdb175a58275b539.png)

## Land\_South/Land\_Core

Land\_South/Land\_Core层基于白盒程序化生成南部区域的地形；

### 生成逻辑

1.  投射地形最低高度（基于**Lowest Height**地形白盒）
    
2.  分别投射 **Unlimited Height** 和 **Limited Height** 两种白盒高度，得到两种白盒的Mask（其中 Unlimited Height 白盒只有在比 Limited Height 白盒高时才会计算投射mask）
    
3.  添加 noise 地形起伏
    
4.  添加随机平台，丰富地形结构
    
5.  Realtime Erosion，添加基础地形侵蚀效果（沉积效果）
    
6.  Gaea Stack 添加更加真实的地形侵蚀沉积效果（较强的侵蚀）
    
7.  由于大侵蚀会破坏 **Limited Height** 的白盒对应的地形高度，因此对 **Limited Height** 白盒再投射一次地形高度
    
8.  添加一些 Distort、Ridge、Rocky 的地形细节，准备整体的小侵蚀；
    
9.  Gaea Stack 添加最后的地形侵蚀效果（小侵蚀，主要用于丰富细节，输出Mask用于给到地形权重）
    
10.  基于侵蚀结果，添加地形权重
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-3f7735964e08a13adb9e05b764d84511.png)

### 更新与适配

如果在场景中，修改了白盒（增加白盒物体，或者修改白盒的Transform），可以执行以下操作来重新计算**投射**和**侵蚀**组件（这两类组件需要手动重新计算，因为如果自动运算，会导致场景编辑非常卡）

1、首先对一开始的几个 MeshProjection 组件进行重新投射

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-43446e5bc08fb55093c2cd21e97e571f.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-45a239db0d303bcdd9d08648467181a0.png)

2、对大侵蚀组件进行重新cook

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-5d49925d8f80d34b182a6daba4a46d30.png)

3、对 Limited Height 白盒进行重新投射

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-e21bd85f6acf5acd112ffad74cd2564e.png)

4、对小侵蚀进行重新 cook

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-798886edd0c6f421f50243ae830d161f.png)

# 白盒输入规范

在程序化组件上，会自动拾取当前场景 **指定关卡** 内 **对应材质实例** 的**所有白盒**进行计算，**在对应的地形区域生成特定地形地貌**，为了防止在计算时出现冲突，需要LD在搭建白盒时，在白盒关卡内根据特定规范用不同的材质实例来表示白盒的具体的含义，这样可以方便后续程序化地生成和调整。

其中**地形（Unlimited Height）**、**地形（Limited Height）**、**地形（Lowest Height）**三种白盒会被程序化层所拾取并进行计算，因此需要**这三种白盒使用特定的材质实例**，其他白盒使用的材质实例与这三种白盒保持不同。

<table data-number-column="false" data-self-adaption="false" data-auto-scale="false" class="render-table"><colgroup><col style="width: 195px;"><col style="width: 268px;"></colgroup><tbody><tr><th rowspan="1" colspan="1" colorname="" data-colwidth="196"><p data-renderer-start-pos="1301" class="paragraph"><strong data-renderer-mark="true">示意对象</strong></p></th><th rowspan="1" colspan="1" colorname="" data-colwidth="269"><p data-renderer-start-pos="1309" class="paragraph"><strong data-renderer-mark="true">材质实例路径</strong></p></th></tr><tr><td rowspan="1" colspan="1" colorname="" data-colwidth="196"><p data-renderer-start-pos="1321" class="paragraph"><strong data-renderer-mark="true">地形（Unlimited Height，表示大概的地形高度，程序化生成的结果只会大致吻合白盒表示）</strong></p></td><td rowspan="1" colspan="1" colorname="" data-colwidth="269"><p data-renderer-start-pos="1374" class="paragraph"><span><span style="display: inline-block; max-width: 100%;" class="ak-renderer-extension-dot" data-local-id="4b5b72a0-327e-4b81-af25-1c0517787349"><span data-extension-type="com.tencent.iwiki.editor.image" data-extension-key="image:stable" data-parameters="{&quot;state&quot;:&quot;stable&quot;,&quot;id&quot;:3757054,&quot;pageId&quot;:4008561772,&quot;name&quot;:&quot;image.png&quot;,&quot;size&quot;:458762,&quot;width&quot;:800,&quot;height&quot;:296,&quot;naturalWidth&quot;:1660,&quot;naturalHeight&quot;:616,&quot;border&quot;:false,&quot;link&quot;:&quot;&quot;,&quot;record&quot;:&quot;middle&quot;}"></span></span></span></p><div style="max-width: 100%;"><div class="extension-dot sc-brqgnP eRdCMn" style="cursor: default;"><div class="border-color-target sc-cMljjf cuEsKD" style="width: 802px;"><div class="iw-attachment-image-root sc-ktHwxA kVCYvz" style="width: 800px; padding: 37% 0px 0px; background: rgb(247, 248, 249);" data-src="/tencent/api/attachments/s3/url?attachmentid=3757054"><div type="none" class="sc-cIShpX kCPGlG"><img class="iwiki-editor-image" src="assets/1704699404-3881bac5ca431182916516be92638f3d.png"></div></div></div></div></div><p></p></td></tr><tr><td rowspan="1" colspan="1" colorname="" data-colwidth="196"><p data-renderer-start-pos="1381" class="paragraph"><strong data-renderer-mark="true">地形（Limited Height，表示准确的地形高度，程序化生成结果会尽可能贴近白盒表示）</strong></p></td><td rowspan="1" colspan="1" colorname="" data-colwidth="269"><p data-renderer-start-pos="1431" class="paragraph"><span><span style="display: inline-block; max-width: 100%;" class="ak-renderer-extension-dot" data-local-id="d1e844dc-5e80-427a-894a-94c270e82eba"><span data-extension-type="com.tencent.iwiki.editor.image" data-extension-key="image:stable" data-parameters="{&quot;state&quot;:&quot;stable&quot;,&quot;id&quot;:3757153,&quot;pageId&quot;:4008561772,&quot;name&quot;:&quot;image.png&quot;,&quot;size&quot;:459013,&quot;width&quot;:800,&quot;height&quot;:300,&quot;naturalWidth&quot;:1645,&quot;naturalHeight&quot;:617,&quot;border&quot;:false,&quot;link&quot;:&quot;&quot;,&quot;record&quot;:&quot;middle&quot;}"></span></span></span></p><div style="max-width: 100%;"><div class="extension-dot sc-brqgnP eRdCMn" style="cursor: default;"><div class="border-color-target sc-cMljjf cuEsKD" style="width: 802px;"><div class="iw-attachment-image-root sc-ktHwxA kVCYvz" style="width: 800px; padding: 38% 0px 0px; background: rgb(247, 248, 249);" data-src="/tencent/api/attachments/s3/url?attachmentid=3757153"><div type="none" class="sc-cIShpX kCPGlG"><img class="iwiki-editor-image" src="assets/1704699404-5d4149f92d39d468daa811d19dd7ddb5.png"></div></div></div></div></div><p></p></td></tr><tr><td rowspan="1" colspan="1" colorname="" data-colwidth="196"><p data-renderer-start-pos="1438" class="paragraph"><strong data-renderer-mark="true">地形（Lowest Height，表示当前区域最低高度；摆放高度不得高于上面两种地形白盒，用于填充无用区域）</strong></p></td><td rowspan="1" colspan="1" colorname="" data-colwidth="269"><p data-renderer-start-pos="1496" class="paragraph"><span><span style="display: inline-block; max-width: 100%;" class="ak-renderer-extension-dot" data-local-id="b147adaf-a8df-4ce5-98e8-ba6b96383ffb"><span data-extension-type="com.tencent.iwiki.editor.image" data-extension-key="image:stable" data-parameters="{&quot;state&quot;:&quot;stable&quot;,&quot;id&quot;:3757214,&quot;pageId&quot;:4008561772,&quot;name&quot;:&quot;image.png&quot;,&quot;size&quot;:454992,&quot;width&quot;:800,&quot;height&quot;:300,&quot;naturalWidth&quot;:1636,&quot;naturalHeight&quot;:614,&quot;border&quot;:false,&quot;link&quot;:&quot;&quot;,&quot;record&quot;:&quot;middle&quot;}"></span></span></span></p><div style="max-width: 100%;"><div class="extension-dot sc-brqgnP eRdCMn" style="cursor: default;"><div class="border-color-target sc-cMljjf cuEsKD" style="width: 802px;"><div class="iw-attachment-image-root sc-ktHwxA kVCYvz" style="width: 800px; padding: 38% 0px 0px; background: rgb(247, 248, 249);" data-src="/tencent/api/attachments/s3/url?attachmentid=3757214"><div type="none" class="sc-cIShpX kCPGlG"><img class="iwiki-editor-image" src="assets/1704699404-6fdeb293340e97eeff9a5c0c506befa9.png"></div></div></div></div></div><p></p></td></tr><tr><td rowspan="1" colspan="1" colorname="" data-colwidth="196"><p data-renderer-start-pos="1503" class="paragraph">河流（程序化层不拾取，纯示意）</p></td><td rowspan="1" colspan="1" colorname="" data-colwidth="269"><p data-renderer-start-pos="1522" class="paragraph"><span><span style="display: inline-block; max-width: 100%;" class="ak-renderer-extension-dot" data-local-id="298f6513-43bb-4251-952e-526018c8a8e6"><span data-extension-type="com.tencent.iwiki.editor.image" data-extension-key="image:stable" data-parameters="{&quot;state&quot;:&quot;stable&quot;,&quot;id&quot;:3757159,&quot;pageId&quot;:4008561772,&quot;name&quot;:&quot;image.png&quot;,&quot;size&quot;:455520,&quot;width&quot;:800,&quot;height&quot;:299,&quot;naturalWidth&quot;:1642,&quot;naturalHeight&quot;:614,&quot;border&quot;:false,&quot;link&quot;:&quot;&quot;,&quot;record&quot;:&quot;middle&quot;}"></span></span></span></p><div style="max-width: 100%;"><div class="extension-dot sc-brqgnP eRdCMn" style="cursor: default;"><div class="border-color-target sc-cMljjf cuEsKD" style="width: 802px;"><div class="iw-attachment-image-root sc-ktHwxA kVCYvz" style="width: 800px; padding: 37% 0px 0px; background: rgb(247, 248, 249);" data-src="/tencent/api/attachments/s3/url?attachmentid=3757159"><div type="none" class="sc-cIShpX kCPGlG"><img class="iwiki-editor-image" src="assets/1704699404-35f20880e6bc58dbd270a2df5662b3e8.png"></div></div></div></div></div><p></p></td></tr><tr><td rowspan="1" colspan="1" colorname="" data-colwidth="196"><p data-renderer-start-pos="1529" class="paragraph">道路（程序化层不拾取，纯示意）</p></td><td rowspan="1" colspan="1" colorname="" data-colwidth="269"><p data-renderer-start-pos="1548" class="paragraph"><span><span style="display: inline-block; max-width: 100%;" class="ak-renderer-extension-dot" data-local-id="f3f8a2ec-e33d-400c-9434-cbd591c1f91c"><span data-extension-type="com.tencent.iwiki.editor.image" data-extension-key="image:stable" data-parameters="{&quot;state&quot;:&quot;stable&quot;,&quot;id&quot;:3757226,&quot;pageId&quot;:4008561772,&quot;name&quot;:&quot;image.png&quot;,&quot;size&quot;:454853,&quot;width&quot;:800,&quot;height&quot;:302,&quot;naturalWidth&quot;:1646,&quot;naturalHeight&quot;:622,&quot;border&quot;:false,&quot;link&quot;:&quot;&quot;,&quot;record&quot;:&quot;middle&quot;}"></span></span></span></p><div style="max-width: 100%;"><div class="extension-dot sc-brqgnP eRdCMn" style="cursor: default;"><div class="border-color-target sc-cMljjf cuEsKD" style="width: 802px;"><div class="iw-attachment-image-root sc-ktHwxA kVCYvz" style="width: 800px; padding: 38% 0px 0px; background: rgb(247, 248, 249);" data-src="/tencent/api/attachments/s3/url?attachmentid=3757226"><div type="none" class="sc-cIShpX kCPGlG"><img class="iwiki-editor-image" src="assets/1704699404-458aed7e99d38a8475b54ce43f6b46ed.png"></div></div></div></div></div><p></p></td></tr><tr><td rowspan="1" colspan="1" colorname="" data-colwidth="196"><p data-renderer-start-pos="1555" class="paragraph">POI（程序化层不拾取，纯示意）</p></td><td rowspan="1" colspan="1" colorname="" data-colwidth="269"><p data-renderer-start-pos="1575" class="paragraph"><span><span style="display: inline-block; max-width: 100%;" class="ak-renderer-extension-dot" data-local-id="5707983d-d4c7-486f-96e9-5bb22bbba121"><span data-extension-type="com.tencent.iwiki.editor.image" data-extension-key="image:stable" data-parameters="{&quot;state&quot;:&quot;stable&quot;,&quot;id&quot;:3757243,&quot;pageId&quot;:4008561772,&quot;name&quot;:&quot;image.png&quot;,&quot;size&quot;:455199,&quot;width&quot;:800,&quot;height&quot;:301,&quot;naturalWidth&quot;:1647,&quot;naturalHeight&quot;:621,&quot;border&quot;:false,&quot;link&quot;:&quot;&quot;,&quot;record&quot;:&quot;middle&quot;}"></span></span></span></p><div style="max-width: 100%;"><div class="extension-dot sc-brqgnP eRdCMn" style="cursor: default;"><div class="border-color-target sc-cMljjf cuEsKD" style="width: 802px;"><div class="iw-attachment-image-root sc-ktHwxA kVCYvz" style="width: 800px; padding: 38% 0px 0px; background: rgb(247, 248, 249);" data-src="/tencent/api/attachments/s3/url?attachmentid=3757243"><div type="none" class="sc-cIShpX kCPGlG"><img class="iwiki-editor-image" src="assets/1704699404-39ea36aae5b7b135b135115e4749f1a3.png"></div></div></div></div></div><p></p></td></tr><tr><td rowspan="1" colspan="1" colorname="" data-colwidth="196"><p data-renderer-start-pos="1582" class="paragraph">围墙（程序化层不拾取，纯示意）</p></td><td rowspan="1" colspan="1" colorname="" data-colwidth="269"><p data-renderer-start-pos="1601" class="paragraph">&nbsp;</p></td></tr></tbody></table>

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

2023-08-14 coenjin

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-48f34ae457aeafc603ab0a403f103594.svg)108
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-0b95f0082c86623bb3ccf41eddc04c8b.png)14
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699404-d56c24c81b5e5f02b023f9382e1ca21d.svg)
