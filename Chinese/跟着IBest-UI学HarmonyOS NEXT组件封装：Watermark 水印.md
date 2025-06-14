# 一、引言：一起来啃源码，解锁HarmonyOS NEXT的“组件密码”！  
嘿，小伙伴们！今天想和大家聊一个超实用的开源项目——**IBest-UI**，一个专为鸿蒙生态打造的轻量级UI组件库。如果你正在开发HarmonyOS NEXT应用，一定遇到过这些痛点：重复造轮子、适配多端界面费时费力、深浅模式切换麻烦……别急，IBest-UI就是来“救场”的！  

**它有多香？**  

+ **轻量到飞起**：核心代码精简，引入即用，绝不给你添负担。  
+ **主题随心换**：深色模式？浅色模式？一行代码切换，适配鸿蒙元服务毫无压力。  
+ **功能小而美**：从按钮到弹窗，从徽章到导航栏，覆盖高频场景，样式参考vant，使用过vant的，都知道vant样式有多好看！

但今天咱们不光是“用组件”，而是要**打开引擎盖，看看里面的“黑科技”**！我们发起一个**源码共读计划**，目标很简单：  

1. **拆解设计思想**：比如TextEllipsis 文本省略，它是怎么实现文本省略的？  
2. **偷师IBest-UI**：在源码中捕捉ArkTS的高阶用法，<font style="color:rgb(37, 41, 51);">学习如何在HarmonyOS NEXT中用声明式UI开发“丝滑”应用。</font>
3. **边学边玩**：欢迎随时抛出问题、提交PR，咱们一起让IBest-UI变得更强大！

无论你是想提升源码阅读能力，还是想摸透鸿蒙开发的门道，这个系列都会是你的“实战指南”。准备好和我一起挖宝了吗？Let’s go！ 🚀  

# 二、准备工作
<font style="color:rgb(89, 89, 89);">看一个开源项目，第一步应该是先看</font><font style="color:rgb(89, 89, 89);"> </font>[<font style="color:rgb(3, 106, 202);">README.md</font>](https://github.com/ibestservices/ibest-ui/blob/master/README.md)<font style="color:rgb(89, 89, 89);"> </font><font style="color:rgb(89, 89, 89);">再看贡献文档</font><font style="color:rgb(89, 89, 89);"> </font>[<font style="color:rgb(3, 106, 202);">github/CONTRIBUTING.md</font>](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fyouzan%2Fvant%2Fblob%2Fmain%2F.github%2FCONTRIBUTING.md)<font style="color:rgb(89, 89, 89);">。</font>

1. **克隆源码**

```javascript
# 克隆gitalb仓库
git clone git@github.com:ibestservices/ibest-ui.git

# 或者克隆gitee仓库
git clone git@gitee.com:ibestservices/ibest-ui.git

# 进入项目
cd ./ibest-ui

# 安装依赖
ohpm install
```

2. **查看目录结构**

根据贡献文档，可以了解到目录结构

```plain
├── entry # 例子hap包
│   └── src
│   ├── main
│   │   ├── ets
│   │   │   ├── assets
│   │   │   │   └── styles # 例子页面样式
│   │   │   ├── components # 例子组件
│   │   │   ├── entryability
│   │   │   └── pages # 例子页面
│   │   └── resources
│   │   ├── base
# ...
├── hvigor
├── library  # 组件库
│   └── src
│   └── main
│   ├── ets
│   │   ├── assets
│   │   │   └── ets # 工具方法
│   │   ├── components # 组件目录
│   │   │   ├── button
│   │   │   ├── cell
│   │   │   └── ...
│   │   └── theme-chalk # 样式变量
│   │   └── src
│   └── resources # 组件库资源
│   ├── base
# ...
```

根据目录，可以了解到，开发的组件、修复bug，主要是在<font style="color:rgb(60, 60, 67);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library</font>` 里进行开发，<font style="color:rgb(60, 60, 67);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">entry/main/ets/pages</font>` 里做组件例子页面。

全局样式变量在<font style="color:rgb(60, 60, 67);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library/src/theme-chalk/...</font>`里定义

# <font style="color:rgb(60, 60, 67);">三、demo 演示</font>
可以通过快捷方式 `Ctrl+Shift+N` 转到文件，输入自己所想找到的文件

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742732392255-1af4e3da-7eb1-44e8-b794-5599af03d933.png)

根据前面的目录结构，我们已经知道了entry是样例文件，那么我们要找的文件，就是在 <font style="color:rgb(60, 60, 67);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">entry\src\main\ets\pages\base\Watermark.ets</font>`里

```typescript
import router from '@ohos.router';
import { IBestButton, IBestNavBar, IBestToast, IBestWatermark } from '@ibestservices/ibest-ui';
import { CONTAINER_SIZE, modeColor, SPACE } from '../../assets/styles/BaseStyle';
import ComponentShowContainer from '../../components/ComponentShowContainer';
import { ComponentRouterParams } from '../../assets/global.type';
@Entry
@Component
struct WatermarkPage {
	@State title: string = (router.getParams() as ComponentRouterParams).title || ''
	@Builder
	content() {
		Column({space: SPACE.SM}) {
			Text($r("app.string.app_desc"))
			IBestButton({
				type: "primary",
				text: "打开弹框",
				onClickBtn: () => {
					IBestToast.show("点击了按钮")
				}
			})
		}
		.width(CONTAINER_SIZE.FULL)
		.height(200)
		.padding(20)
		.justifyContent(FlexAlign.Center)
	}
	build() {
		Column() {
			IBestNavBar({
				title: this.title,
				isShowStatusBar: true,
				onLeftClick: () => {
					router.back()
				}
			})
			List() {
				ListItem() {
					ComponentShowContainer({ title: '文字水印', titlePaddingLeft: SPACE.XS }) {
						IBestWatermark({
							text: 'IBest-UI',
							gapX: 40,
							gapY: 40
						}) {
							this.content()
						}
					}
				}
				ListItem() {
					ComponentShowContainer({ title: '图片水印', titlePaddingLeft: SPACE.XS }) {
						Column({space: SPACE.SM}){
							IBestWatermark({
								imageUrl: $r("app.media.app_icon"),
								waterMarkWidth: 40,
								waterMarkHeight: 40,
								gapX: 40,
								gapY: 40,
								rotateDeg: 0
							}) {
								this.content()
							}
							IBestWatermark({
								imageUrl: 'https://fastly.jsdelivr.net/npm/@vant/assets/logo.png',
								waterMarkWidth: 40,
								waterMarkHeight: 40,
								gapX: 40,
								gapY: 40,
								rotateDeg: 30
							}) {
								this.content()
							}
						}
					}
				}
				ListItem() {
					ComponentShowContainer({ title: '自定义间隔', titlePaddingLeft: SPACE.XS }) {
						IBestWatermark({
							text: $r("app.string.app_name"),
							gapX: 80,
							gapY: 80
						}) {
							this.content()
						}
					}
				}
				ListItem() {
					ComponentShowContainer({ title: '自定义倾斜角度', titlePaddingLeft: SPACE.XS }) {
						IBestWatermark({
							text: $r("app.string.app_name"),
							fontSize: 14,
							gapX: 20,
							gapY: 20,
							rotateDeg: 0
						}) {
							this.content()
						}
					}
				}
				ListItem() {
					ComponentShowContainer({ title: '自定义层级', titlePaddingLeft: SPACE.XS }) {
						IBestWatermark({
							text: $r("app.string.app_name"),
							gapX: 40,
							gapY: 40,
							waterMarkZIndex: 1
						}) {
							this.content()
						}
					}
				}
			}
			.layoutWeight(1)
			.padding({ bottom: SPACE.SM })
		}
		.width(CONTAINER_SIZE.FULL)
		.height(CONTAINER_SIZE.FULL)
		.backgroundColor(modeColor.bg)
	}
}
```

运行下demo，看下demo使用效果

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742733661438-fd9dd72a-9080-43ef-91af-5f7fb6394f29.png)![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742733696403-73a6df0a-02bf-4fa9-a63b-2bcda932f55a.png)

# 四、源码解析
## <font style="color:rgb(60, 60, 67);">快速找到源代码位置</font>
可以通过快捷方式 `Ctrl+Shift+N` 转到文件，输入自己所想找到的文件

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742732410818-ff52aaaa-76d1-40a1-a98b-b70c400feab4.png)

根据前面的目录结构，我们已经知道了entry是样例文件，library是组件库文件，那么我们要找的文件，就是在 <font style="color:rgb(60, 60, 67);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\ets\components\watermark</font>`里

进到文件里，我们可以看到有三个文件

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742733781014-fd8e9c8c-9679-4ba4-bb8b-325fb2cbcd40.png)

color.est文件定义了相关样式，可以看到，在 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\resources\base\element\color.json</font>` 读取样式，好处理全局样式

```javascript
interface IBestWatermarkColorType {
    bgColor: ResourceColor
}

export const IBestWatermarkColor: IBestWatermarkColorType = {
    bgColor: $r("app.color.ibest_water_mark_background")
}
```

### index.est代码解释
接下来就看下主文件<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">index.est</font>（已添加相关注释）

```javascript
/**
 * 导入必要的类型和工具函数
 */
import { IBestStringNumber } from '../../model/Global.type'
import { getDefaultBaseStyle, IBEST_UI_NAMESPACE } from '../../theme-chalk/src'
import { IBestUIBaseStyleObjType } from '../../theme-chalk/src/index.type'
import { IBestWatermarkColor } from './color'
import { WatermarkCanvas } from './watermarkCanvas'

/**
 * 定义一个水印组件，支持图片水印和文字水印
 */
@Component
export struct IBestWatermark {
    /**
     * 全局公共样式，从存储中加载默认主题样式
     */
    @StorageLink(IBEST_UI_NAMESPACE) baseStyle: IBestUIBaseStyleObjType = getDefaultBaseStyle()
    /**
     * 默认的插槽内容，用于放置用户自定义的内容
     */
    @BuilderParam defaultSlot?: CustomBuilder
    /**
     * 图片水印的宽度
     */
    @Prop waterMarkWidth: number
    /**
     * 图片水印的高度
     */
    @Prop waterMarkHeight: number
    /**
     * 水印透明度，默认值为0.3
     */
    @Prop waterMarkOpacity: number = 0.3
    /**
     * 水印之间的水平间隔，默认值为0
     */
    @Prop gapX: number
    /**
     * 水印之间的垂直间隔，默认值为0
     */
    @Prop gapY: number
    /**
     * 水印旋转角度
     */
    @Prop rotateDeg: number
    /**
     * 图片水印的链接
     */
    @Prop imageUrl: ResourceStr
    /**
     * 文字水印内容
     */
    @Prop text: ResourceStr
    /**
     * 文字水印的字体大小
     */
    @Prop fontSize: number
    /**
     * 文字水印的字体类型
     */
    @Prop fontFamily: string
    /**
     * 文字水印的颜色，支持渐变或图案
     */
    @Prop fontColor: IBestStringNumber | CanvasGradient | CanvasPattern
    /**
     * 水印的z-index，默认值为-1（在背景层）
     */
    @Prop waterMarkZIndex: number = -1
    /**
     * 背景色，默认为空
     * @since 2.0.1
     */
    @Prop bgColor: ResourceColor = ""
    /**
     * 水印区域的总高度，动态计算
     */
    @State waterMarkContainHeight: number = 0
    /**
     * 水印区域的总宽度，动态计算
     */
    @State waterMarkContainWidth: number = 0
    /**
     * 水印图片的URL，由WatermarkCanvas生成
     */
    @State url: string = ''
    /**
     * 定义水印内容的构建器
     */
    @Builder watermarkContent() {
        Image(this.url)
            .width(this.waterMarkContainWidth) // 设置水印图片宽度
            .height(this.waterMarkContainHeight) // 设置水印图片高度
            .opacity(this.waterMarkOpacity) // 设置水印透明度
    }
    /**
     * 获取背景色，优先使用用户定义的bgColor，否则使用默认值
     */
    getBgColor(): ResourceColor {
        return this.bgColor || IBestWatermarkColor.bgColor
    }
    /**
     * 构建组件UI
     */
    build() {
        Stack() { // 使用Stack布局容器
            WatermarkCanvas({ // 调用WatermarkCanvas生成水印图片
                waterMarkContainHeight: this.waterMarkContainHeight, // 水印区域高度
                waterMarkContainWidth: this.waterMarkContainWidth, // 水印区域宽度
                waterMarkHeight: this.waterMarkHeight, // 单个水印高度
                waterMarkWidth: this.waterMarkWidth, // 单个水印宽度
                fontFamily: this.fontFamily, // 字体类型
                rotateDeg: this.rotateDeg, // 旋转角度
                fontColor: this.fontColor, // 字体颜色
                imageUrl: this.imageUrl, // 图片水印链接
                fontSize: this.fontSize, // 字体大小
                gapY: this.gapY, // 垂直间隔
                gapX: this.gapX, // 水平间隔
                text: this.text, // 文字水印内容
                onComplete: (url: string) => { // 水印生成完成后的回调
                    this.url = url // 更新水印图片URL
                }
            })

            if (this.waterMarkZIndex <= 0) { // 如果z-index小于等于0，将水印放在背景层
                this.watermarkContent()
            }

            Column() { // 定义一个Column容器，用于放置默认插槽内容
                if (this.defaultSlot) { // 如果存在默认插槽内容，则渲染
                    this.defaultSlot()
                }
            }
            .onAreaChange((oldValue: Area, newValue: Area) => { // 监听区域变化，动态更新水印区域尺寸
                this.waterMarkContainHeight = newValue.height as number
                this.waterMarkContainWidth = newValue.width as number
            })

            if (this.waterMarkZIndex > 0) { // 如果z-index大于0，将水印放在前景层
                this.watermarkContent()
            }
        }
        .backgroundColor(this.getBgColor()) // 设置背景色
    }
}
```

以下是对`IBestWatermark`组件的详细解释，分为几个主要部分进行说明：

#### **1. 导入模块**
```arkts
import { IBestStringNumber } from '../../model/Global.type'
import { getDefaultBaseStyle, IBEST_UI_NAMESPACE } from '../../theme-chalk/src'
import { IBestUIBaseStyleObjType } from '../../theme-chalk/src/index.type'
import { IBestWatermarkColor } from './color'
import { WatermarkCanvas } from './watermarkCanvas'
```

+ **功能**：导入了必要的类型和工具函数。
    - `IBestStringNumber`：定义了一种支持字符串或数字的类型。
    - `getDefaultBaseStyle` 和 `IBEST_UI_NAMESPACE`：用于加载全局主题样式。
    - `IBestWatermarkColor`：定义了水印组件的颜色相关配置。
    - `WatermarkCanvas`：负责生成水印图片的核心组件。

#### **2. 属性定义**
以下是`IBestWatermark`组件中定义的属性及其作用：

| 属性名 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `baseStyle` | `IBestUIBaseStyleObjType` |  | 全局公共样式，从存储中加载默认主题样式。 |
| `defaultSlot` | `CustomBuilder` |  | 默认插槽内容，用于放置用户自定义的内容。 |
| `waterMarkWidth` | `number` |  | 图片水印的宽度。 |
| `waterMarkHeight` | `number` |  | 图片水印的高度。 |
| `waterMarkOpacity` | `number` | `0.3` | 水印透明度，默认值为`0.3`。 |
| `gapX` | `number` |  | 水印之间的水平间隔，默认值为`0`。 |
| `gapY` | `number` |  | 水印之间的垂直间隔，默认值为`0`。 |
| `rotateDeg` | `number` |  | 水印旋转角度。 |
| `imageUrl` | `ResourceStr` |  | 图片水印的链接。 |
| `text` | `ResourceStr` |  | 文字水印内容。 |
| `fontSize` | `number` |  | 文字水印的字体大小。 |
| `fontFamily` | `string` |  | 文字水印的字体类型。 |
| `fontColor` | `IBestStringNumber | CanvasGradient | CanvasPattern` |
| `waterMarkZIndex` | `number` | `-1` | 水印的z-index，默认值为`-1`（在背景层）。 |
| `bgColor` | `ResourceColor` | `""` | 背景色，默认为空。 |
| `waterMarkContainHeight` | `number` | `0` | 水印区域的总高度，动态计算。 |
| `waterMarkContainWidth` | `number` | `0` | 水印区域的总宽度，动态计算。 |
| `url` | `string` | `''` | 水印图片的URL，由`WatermarkCanvas`生成。 |


#### **4. 方法定义**
##### **(1) **`watermarkContent`
```arkts
@Builder watermarkContent() {
    Image(this.url)
        .width(this.waterMarkContainWidth)
        .height(this.waterMarkContainHeight)
        .opacity(this.waterMarkOpacity)
}
```

+ **功能**：定义水印内容的构建器。
+ **逻辑**：
    - 使用`Image`组件加载水印图片`this.url`。
    - 设置水印图片的宽度、高度和透明度。

#### **5. 构建UI (**`build`**方法)**
```arkts
build() {
    Stack() { 
        WatermarkCanvas({ 
            waterMarkContainHeight: this.waterMarkContainHeight,
            waterMarkContainWidth: this.waterMarkContainWidth,
            waterMarkHeight: this.waterMarkHeight,
            waterMarkWidth: this.waterMarkWidth,
            fontFamily: this.fontFamily,
            rotateDeg: this.rotateDeg,
            fontColor: this.fontColor,
            imageUrl: this.imageUrl,
            fontSize: this.fontSize,
            gapY: this.gapY,
            gapX: this.gapX,
            text: this.text,
            onComplete: (url: string) => { 
                this.url = url 
            }
        })

        if (this.waterMarkZIndex <= 0) { 
            this.watermarkContent()
        }

        Column() { 
            if (this.defaultSlot) { 
                this.defaultSlot()
            }
        }
        .onAreaChange((oldValue: Area, newValue: Area) => { 
            this.waterMarkContainHeight = newValue.height as number
            this.waterMarkContainWidth = newValue.width as number
        })

        if (this.waterMarkZIndex > 0) { 
            this.watermarkContent()
        }
    }
    .backgroundColor(this.getBgColor())
}
```

+ **功能**：构建组件的UI布局。
+ **逻辑**：
    1. **Stack布局容器**：
        * 使用`Stack`作为根容器。
    2. **调用**`WatermarkCanvas`**生成水印图片**：
        * 将所有水印相关的属性传递给`WatermarkCanvas`。
        * <font style="color:#DF2A3F;">当水印生成完成后，通过</font>`<font style="color:#DF2A3F;">onComplete</font>`<font style="color:#DF2A3F;">回调更新</font>`<font style="color:#DF2A3F;">this.url</font>`<font style="color:#DF2A3F;">。</font>
    3. **根据**`z-index`**决定水印位置**：
        * 如果`z-index <= 0`，将水印放在背景层。
        * 如果`z-index > 0`，将水印放在前景层。
    4. **Column容器**：
        * 定义一个`Column`容器，用于放置默认插槽内容。
        * <font style="color:#DF2A3F;">监听区域变化事件，动态更新水印区域的宽度和高度</font>。
    5. **设置背景色**：
        * 调用`getBgColor`方法设置背景色。

![](https://cdn.nlark.com/yuque/__mermaid_v3/ccf229f124ca4880aa035a398198f1ab.svg)

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742993324324-fd34ec48-90e1-4375-afbe-e1874c960568.png)

#### **6. 总结**
+ **核心功能**：`IBestWatermark`组件实现了灵活的水印功能，支持图片水印和文字水印，并允许用户自定义水印的样式、位置和透明度。
+ **动态特性**：通过监听区域变化事件，动态调整水印区域的尺寸。
+ **扩展性**：支持通过插槽插入用户自定义内容，增强了组件的灵活性和可复用性。

### WatermarkCanvas.est代码介绍
接下来就看下文件<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">WatermarkCanvas.est</font>（已添加相关注释）

```javascript
import { IBestStringNumber } from '../../model/Global.type'
import GlobalStore from '../../utils/GlobalStore'
import { base64ToPixelMap, getResourceStr } from '../../utils/utils'

interface DrawCanvasItemArgs {
    height: number,
    width: number,
    rotateDeg: number,
    callback: () => void
}
interface CountData {
    countX: number,
    countY: number
}
/**
 * WatermarkCanvas组件用于生成和管理水印的绘制
 */
@Component
export struct WatermarkCanvas {
  @StorageProp("IBestColorMode") @Watch("drawWaterMark") colorMode: ColorMode = ColorMode.LIGHT
  settings: RenderingContextSettings = new RenderingContextSettings(true)
  context: CanvasRenderingContext2D = new CanvasRenderingContext2D(this.settings)
  /**
   * 图片水印的宽度
   */
  @Prop @Watch('drawWaterMark') waterMarkWidth: number = 100
  /**
   * 图片水印的高度
   */
  @Prop @Watch('drawWaterMark') waterMarkHeight: number = 100
  /**
   * 水印透明度
   */
  @Prop waterMarkOpacity: number = 0.3
  /**
   * 旋转角度
   */
  @Prop @Watch('drawWaterMark') rotateDeg: number = -22
  /**
   * 水印区域的总高度
   */
  @Prop @Watch('drawWaterMark') waterMarkContainHeight: number = 0
  /**
   * 水印区域的总宽度
   */
  @Prop @Watch('drawWaterMark') waterMarkContainWidth: number = 0
  /**
   * 水印之间的水平间隔 默认0
   */
  @Prop @Watch('drawWaterMark') gapX: number = 0
  /**
   * 水印之间的垂直间隔 默认0
   */
  @Prop @Watch('drawWaterMark') gapY: number = 0
  /**
   * 图片水印的链接
   */
  @Prop @Watch('drawWaterMark') imageUrl: ResourceStr = ''
  /**
   * 文字水印的文案 文字水印优先级高于图片水印
   */
  @Prop @Watch('drawWaterMark') text: ResourceStr = ''
  /**
   * 文字的大小
   */
  @Prop @Watch('drawWaterMark') fontSize: number = 14
  /**
   * 文字字体
   */
  @Prop @Watch('drawWaterMark') fontFamily: string = 'sans-serif'
  /**
   * 文字颜色
   */
  @Prop @Watch('drawWaterMark') fontColor: IBestStringNumber | CanvasGradient | CanvasPattern = ''
  /**
   * 绘制完成回调
   */
  onComplete: (url: string) => void = () => {}
  private resourceManager = GlobalStore.context.resourceManager
  /**
   * 获取文字颜色，如果没有设置，则根据colorMode选择默认颜色
   * @returns {string} 颜色值
   */
  getFontColor() {
      return this.fontColor || (this.colorMode == 0 ? "#f5f5f5" : "#323232")
  }
  /**
   * 获取文字水印的文案
   * @returns {string} 文案
   */
  getText(){
      return getResourceStr(this.text)
  }
  // 清除画布
  clearCanvas() {
      this.context?.clearRect(0, 0, this.waterMarkWidth, this.waterMarkHeight)
  }
  /**
   * 获取x轴与y轴的绘制数量
   * @param width 每个水印的宽度
   * @param height 每个水印的高度
   * @returns {CountData} 包含x轴和y轴的绘制数量
   */
  getXYCount(width: number, height: number) {
      // 计算出水平数量
      let countX = Math.ceil(this.waterMarkContainWidth / (width + this.gapX))
      // 保证最少有一个
      if (countX <= 0) {
          countX = 1
      }
      // 计算出垂直数量
      let countY = Math.ceil(this.waterMarkContainHeight / (height + this.gapY))
      // 保证最少有一个
      if (countY <= 0) {
          countY = 1
      }
      // 避免因为旋转导致有部分留白 所以多渲染点
      countY = countY + 2
      countX = countX + 2
      const countData: CountData = {
          countX,
          countY
      }
      return countData
  }
  /**
   * 创建矩阵绘制canvas
   * @param {number} width 每个绘制块的宽度
   * @param {number} height 每个绘制块的高度
   * @param {number} rotateDeg 旋转的角度
   * @param {()=>void} callback 每次绘制的内容
   */
  drawCanvasItem(data: DrawCanvasItemArgs) {
      const width = data.width
      const height = data.height
      const rotateDeg = data.rotateDeg
      const callback = data.callback
      const countData = this.getXYCount(width, height)
      const countX = countData.countX
      const countY = countData.countY
      // 根据计算出的数量创建矩阵
      // i是Y轴
      for (let i = 0; i < countY; i++) {
          // j是X轴
          for (let j = 0; j < countX; j++) {
              const x = width * j + this.gapX * j
              const y = height * i + this.gapY * i
              this.context.save()
              this.context.translate(x, y)
              this.context.rotate(rotateDeg)
              callback()
              // 还原之前保存的状态
              this.context.restore()
          }
      }
      let url: string = this.context.toDataURL("png")
      if (url != "data:image/png") {
          this.onComplete(url)
      }
  }
  /**
   * 绘制图片水印
   * @param {number} rotateDeg 旋转的角度
   */
  async handleLayoutImg(rotateDeg: number) {
      let imgModel: PixelMap | ImageBitmap
      if (typeof this.imageUrl == 'string') {
          if(this.imageUrl.startsWith("data:image")){ // base64
              imgModel = await base64ToPixelMap(this.imageUrl)
          } else { // 本地选择的图片 网络图片
              imgModel = new ImageBitmap(this.imageUrl)
          }
      } else {	// Resource
          imgModel = await base64ToPixelMap(this.resourceManager.getMediaContentBase64Sync(this.imageUrl))
      }
      this.drawCanvasItem({
          width: this.waterMarkWidth,
          height: this.waterMarkHeight,
          rotateDeg,
          callback: () => {
              this.context.drawImage(imgModel, 0, 0, this.waterMarkWidth, this.waterMarkHeight)
          }
      })
  }
  /**
   * 绘制文字水印
   * @param {number} rotateDeg 旋转的角度
   */
  handleLayoutText(rotateDeg: number) {
      let text = this.getText()
      this.context.fillStyle = this.getFontColor()
      this.context.font = `${vp2px(this.fontSize)}px ${this.fontFamily}`
      const data = this.context.measureText(text)
      this.drawCanvasItem({
          width: data.width,
          height: data.height,
          rotateDeg,
          callback: () => {
              this.context.fillText(text, 0, 0)
          }
      })
  }
  /**
   * 绘制水印
   */
  drawWaterMark() {
      this.clearCanvas()
      this.context.textBaseline = 'top'
      const rotateDeg = this.rotateDeg * Math.PI / 180
      if(this.text){
          this.handleLayoutText(rotateDeg)
      }else if(this.imageUrl){
          this.handleLayoutImg(rotateDeg)
      }
  }
  /**
   * 构建Canvas组件
   */
  build() {
      Canvas(this.context)
          .width(this.waterMarkContainWidth)
          .height(this.waterMarkContainHeight)
          .onReady(() => {
              this.drawWaterMark()
          })
          .opacity(0)
          .enabled(false)
  }
}
```

以下是`WatermarkCanvas`组件的详细解释，分为几个主要部分进行说明：

#### **1. 导入模块**
```arkts
import { IBestStringNumber } from '../../model/Global.type'
import GlobalStore from '../../utils/GlobalStore'
import { base64ToPixelMap, getResourceStr } from '../../utils/utils'
```

+ **功能**：导入了必要的类型和工具函数。
    - `IBestStringNumber`：定义了一种支持字符串或数字的类型。
    - `GlobalStore`：用于访问全局存储。
    - `base64ToPixelMap` 和 `getResourceStr`：工具函数，分别用于将Base64字符串转换为像素映射和获取资源字符串。

---

#### **2. 定义接口**
```arkts
interface DrawCanvasItemArgs {
    height: number,
    width: number,
    rotateDeg: number,
    callback: () => void
}
interface CountData {
    countX: number,
    countY: number
}
```

+ **功能**：定义了两个接口，用于类型检查。
    - `DrawCanvasItemArgs`：用于传递绘制单个水印块的参数。
    - `CountData`：用于存储计算出的水平和垂直方向上的水印数量。

#### **3. 定义**`WatermarkCanvas`**组件**
```arkts
/**
 * WatermarkCanvas组件用于生成和管理水印的绘制
 */
@Component
export struct WatermarkCanvas {
    ...
}
```

+ **功能**：定义了一个名为`WatermarkCanvas`的组件，负责生成和管理水印的绘制。

#### **4. 属性定义**
![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742994180169-366b41d2-5f88-4a1e-b2ab-9505af7256a8.png)

以下是`WatermarkCanvas`组件中定义的属性及其作用：

| 属性名 | 类型 | 默认值 | 描述 |
| --- | --- | --- | --- |
| `colorMode` | `ColorMode` | `ColorMode.LIGHT` | 全局颜色模式，从存储中加载。 |
| `settings` | `RenderingContextSettings` |  | 渲染上下文设置。 |
| `context` | `CanvasRenderingContext2D` |  | 2D画布渲染上下文。 |
| `waterMarkWidth` | `number` | `100` | 图片水印的宽度。 |
| `waterMarkHeight` | `number` | `100` | 图片水印的高度。 |
| `waterMarkOpacity` | `number` | `0.3` | 水印透明度，默认值为`0.3`。 |
| `rotateDeg` | `number` | `-22` | 水印旋转角度。 |
| `waterMarkContainHeight` | `number` | `0` | 水印区域的总高度。 |
| `waterMarkContainWidth` | `number` | `0` | 水印区域的总宽度。 |
| `gapX` | `number` | `0` | 水印之间的水平间隔，默认值为`0`。 |
| `gapY` | `number` | `0` | 水印之间的垂直间隔，默认值为`0`。 |
| `imageUrl` | `ResourceStr` | `''` | 图片水印的链接。 |
| `text` | `ResourceStr` | `''` | 文字水印内容。 |
| `fontSize` | `number` | `14` | 文字水印的字体大小。 |
| `fontFamily` | `string` | `'sans-serif'` | 文字水印的字体类型。 |
| `fontColor` | `IBestStringNumber | CanvasGradient | CanvasPattern` |
| `onComplete` | `(url: string) => void` | `() => {}` | 水印生成完成后的回调函数。 |
| `resourceManager` | `GlobalStore.context.resourceManager` |  | 资源管理器，用于获取资源内容。 |


#### **5. 方法定义**
##### **(1) **`getFontColor`
```arkts
/**
 * 获取文字颜色，如果没有设置，则根据colorMode选择默认颜色
 * @returns {string} 颜色值
 */
getFontColor() {
    return this.fontColor || (this.colorMode == 0 ? "#f5f5f5" : "#323232")
}
```

+ **功能**：获取文字颜色。
+ **逻辑**：
    - 如果`fontColor`已设置，则返回`fontColor`。
    - 否则根据`colorMode`选择默认颜色：
        * `colorMode == 0`：返回`#f5f5f5`。
        * 否则返回`#323232`。

##### **(2) **`getText`
```arkts
/**
 * 获取文字水印的文案
 * @returns {string} 文案
 */
getText(){
    return getResourceStr(this.text)
}
```

+ **功能**：获取文字水印的文案。
+ **逻辑**：
    - 调用`getResourceStr`函数获取资源字符串`this.text`。

##### **(3) **`clearCanvas`
```arkts
// 清除画布
clearCanvas() {
    this.context?.clearRect(0, 0, this.waterMarkWidth, this.waterMarkHeight)
}
```

+ **功能**：清除画布上的内容。
+ **逻辑**：
    - 使用`clearRect`方法清除指定区域的内容。

##### **(4) **`getXYCount`
```arkts
/**
 * 获取x轴与y轴的绘制数量
 * @param width 每个水印的宽度
 * @param height 每个水印的高度
 * @returns {CountData} 包含x轴和y轴的绘制数量
 */
getXYCount(width: number, height: number) {
    // 计算出水平数量
    let countX = Math.ceil(this.waterMarkContainWidth / (width + this.gapX))
    // 保证最少有一个
    if (countX <= 0) {
        countX = 1
    }
    // 计算出垂直数量
    let countY = Math.ceil(this.waterMarkContainHeight / (height + this.gapY))
    // 保证最少有一个
    if (countY <= 0) {
        countY = 1
    }
    // 避免因为旋转导致有部分留白 所以多渲染点
    countY = countY + 2
    countX = countX + 2
    const countData: CountData = {
        countX,
        countY
    }
    return countData
}
```

+ **功能**：计算在给定区域内绘制水印所需的水平和垂直数量。
+ **逻辑**：
    1. **计算水平数量 (**`countX`**)**：
        * 根据`waterMarkContainWidth`、`width`和`gapX`计算水平方向上的水印数量。
        * 确保至少有一个水印。
    2. **计算垂直数量 (**`countY`**)**：
        * 根据`waterMarkContainHeight`、`height`和`gapY`计算垂直方向上的水印数量。
        * 确保至少有一个水印。
    3. **避免留白**：
        * 为了防止旋转导致的部分留白，增加`countY`和`countX`各2个单位。
    4. **返回结果**：
        * 返回包含`countX`和`countY`的对象。

##### **(5) **`drawCanvasItem`
```arkts
/**
 * 创建矩阵绘制canvas
 * @param {number} width 每个绘制块的宽度
 * @param {number} height 每个绘制块的高度
 * @param {number} rotateDeg 旋转的角度
 * @param {()=>void} callback 每次绘制的内容
 */
drawCanvasItem(data: DrawCanvasItemArgs) {
    const width = data.width
    const height = data.height
    const rotateDeg = data.rotateDeg
    const callback = data.callback
    const countData = this.getXYCount(width, height)
    const countX = countData.countX
    const countY = countData.countY
    // 根据计算出的数量创建矩阵
    // i是Y轴
    for (let i = 0; i < countY; i++) {
        // j是X轴
        for (let j = 0; j < countX; j++) {
            const x = width * j + this.gapX * j
            const y = height * i + this.gapY * i
            this.context.save()
            this.context.translate(x, y)
            this.context.rotate(rotateDeg)
            callback()
            // 还原之前保存的状态
            this.context.restore()
        }
    }
    let url: string = this.context.toDataURL("png")
    if (url != "data:image/png") {
        this.onComplete(url)
    }
}
```

+ **功能**：在画布上绘制多个水印块。
+ **逻辑**：
    1. **获取参数**：
        * 从`data`对象中提取`width`、`height`、`rotateDeg`和`callback`。
    2. **计算绘制数量**：
        * 调用`getXYCount`方法获取水平和垂直方向上的绘制数量`countX`和`countY`。
    3. **绘制矩阵**：
        * 使用嵌套的`for`循环遍历每个水印块的位置。
        * 计算每个水印块的`x`和`y`坐标。
        * 保存当前的渲染状态。
        * 平移和旋转画布到指定位置。
        * 调用`callback`函数执行具体的绘制操作。
        * 还原之前的渲染状态。
    4. **生成数据URL**：
        * 将绘制好的画布内容转换为PNG格式的数据URL。
        * 如果生成的URL不是默认的`data:image/png`，则调用`onComplete`回调函数并传递生成的URL。

##### **(6) **`handleLayoutImg`
```arkts
/**
 * 绘制图片水印
 * @param {number} rotateDeg 旋转的角度
 */
async handleLayoutImg(rotateDeg: number) {
    let imgModel: PixelMap | ImageBitmap
    if (typeof this.imageUrl == 'string') {
        if(this.imageUrl.startsWith("data:image")){ // base64
            imgModel = await base64ToPixelMap(this.imageUrl)
        } else { // 本地选择的图片 网络图片
            imgModel = new ImageBitmap(this.imageUrl)
        }
    } else {	// Resource
        imgModel = await base64ToPixelMap(this.resourceManager.getMediaContentBase64Sync(this.imageUrl))
    }
    this.drawCanvasItem({
        width: this.waterMarkWidth,
        height: this.waterMarkHeight,
        rotateDeg,
        callback: () => {
            this.context.drawImage(imgModel, 0, 0, this.waterMarkWidth, this.waterMarkHeight)
        }
    })
}
```

+ **功能**：绘制图片水印。
+ **逻辑**：
    1. **加载图片模型**：
        * 根据`imageUrl`的类型加载图片模型。
        * 如果`imageUrl`是字符串：
            + 如果以`data:image`开头，调用`base64ToPixelMap`将Base64字符串转换为`PixelMap`。
            + 否则，创建`ImageBitmap`对象。
        * 如果`imageUrl`是资源对象，调用`resourceManager.getMediaContentBase64Sync`获取Base64字符串，再转换为`PixelMap`。
    2. **绘制图片**：
        * 调用`drawCanvasItem`方法，传递图片的宽度、高度、旋转角度和绘制回调函数。
        * 在回调函数中，使用`drawImage`方法在指定位置绘制图片。

##### **(7) **`handleLayoutText`
```arkts
/**
 * 绘制文字水印
 * @param {number} rotateDeg 旋转的角度
 */
handleLayoutText(rotateDeg: number) {
    let text = this.getText()
    this.context.fillStyle = this.getFontColor()
    this.context.font = `${vp2px(this.fontSize)}px ${this.fontFamily}`
    const data = this.context.measureText(text)
    this.drawCanvasItem({
        width: data.width,
        height: data.height,
        rotateDeg,
        callback: () => {
            this.context.fillText(text, 0, 0)
        }
    })
}
```

+ **功能**：绘制文字水印。
+ **逻辑**：
    1. **获取文字和颜色**：
        * 调用`getText`方法获取文字内容。
        * 调用`getFontColor`方法获取文字颜色。
    2. **设置字体样式**：
        * 设置`fillStyle`为文字颜色。
        * 设置`font`属性为字体大小和字体类型。
    3. **测量文字宽度**：
        * 使用`measureText`方法测量文字的宽度。
    4. **绘制文字**：
        * 调用`drawCanvasItem`方法，传递文字的宽度、高度、旋转角度和绘制回调函数。
        * 在回调函数中，使用`fillText`方法在指定位置绘制文字。

##### **(8) **`drawWaterMark`
```arkts
/**
 * 绘制水印
 */
drawWaterMark() {
    this.clearCanvas()
    this.context.textBaseline = 'top'
    const rotateDeg = this.rotateDeg * Math.PI / 180
    if(this.text){
        this.handleLayoutText(rotateDeg)
    }else if(this.imageUrl){
        this.handleLayoutImg(rotateDeg)
    }
}
```

+ **功能**：根据配置绘制水印。
+ **逻辑**：
    1. **清除画布**：
        * 调用`clearCanvas`方法清除画布上的内容。
    2. **设置文本基线**：
        * 设置`textBaseline`为`'top'`，确保文字从顶部对齐。
    3. **转换旋转角度**：
        * 将`rotateDeg`从度数转换为弧度。
    4. **绘制水印**：
        * 如果`text`属性存在，调用`handleLayoutText`方法绘制文字水印。
        * 否则如果`imageUrl`属性存在，调用`handleLayoutImg`方法绘制图片水印。

##### **(9) **`build`
```arkts
/**
 * 构建Canvas组件
 */
build() {
    Canvas(this.context)
        .width(this.waterMarkContainWidth)
        .height(this.waterMarkContainHeight)
        .onReady(() => {
            this.drawWaterMark()
        })
        .opacity(0)
        .enabled(false)
}
```

+ **功能**：构建Canvas组件并设置其属性。
+ **逻辑**：
    1. **创建Canvas组件**：
        * 使用`Canvas`组件并传入`context`。
    2. **设置宽度和高度**：
        * 设置Canvas的宽度为`waterMarkContainWidth`。
        * 设置Canvas的高度为`waterMarkContainHeight`。
    3. **设置就绪回调**：
        * 在Canvas准备好后，调用`drawWaterMark`方法绘制水印。
    4. **设置透明度和启用状态**：
        * 设置Canvas的透明度为`0`，使其不可见。
        * 设置Canvas的启用状态为`false`，使其不可交互。

#### 6. 流程图
![](https://cdn.nlark.com/yuque/__mermaid_v3/e911fc6e2dd2599bee2f766ad33924e8.svg)

![](https://cdn.nlark.com/yuque/0/2025/svg/26122809/1742995308408-ea62616d-4d17-4670-8bc1-a1344a473ce0.svg)

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1743089622594-048ef03d-f860-4209-abb0-fa195ff1a42d.png)

#### **7. 总结**
+ **核心功能**：`WatermarkCanvas`组件负责生成和管理水印的绘制，支持图片水印和文字水印。
+ **动态特性**：根据传入的属性动态计算水印的数量和位置，并支持旋转和透明度设置。
+ **扩展性**：通过回调函数`onComplete`，允许外部处理生成的水印图片。

# 五. 总结
### **快速回顾：Watermark 组件核心亮点**
1. **动态渲染引擎**  
    - 通过 `Canvas` 实现文字/图片水印的矩阵排列，支持旋转、透明度、间隔动态调节。  
    - 自动适配容器尺寸变化（`onAreaChange` 监听），实时更新布局。
2. **主题与资源兼容**  
    - 深浅模式自动切换水印颜色，兼容本地资源、网络图片、Base64 格式。  
    - 通过 `@StorageLink` 实时响应全局主题变更。
3. **性能优化设计**  
    - 仅在 `Canvas` 就绪后绘制（`onReady`），避免白屏。  
    - 通过 `toDataURL` 生成静态图片，减少重复绘制开销。
4. **开发者友好**  
    - 属性驱动配置（`gapX`/`gapY`/`rotateDeg`），支持插槽嵌入自定义内容。  
    - 源码模块化清晰（`WatermarkCanvas` 负责绘制逻辑，`IBestWatermark` 负责 UI 组合）。

**一句话总结**：IBest-UI 的 Watermark 组件通过声明式 API 和 Canvas 高效渲染，为鸿蒙开发者提供了“零负担”的水印解决方案，是学习 ArkTS 进阶技巧的优质案例。



#### 
