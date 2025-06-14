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

根据目录，可以了解到，开发的组件、修复bug，主要是在library里进行开发，entry/main/ets/pages里做组件例子页面。

全局样式变量在<font style="color:rgb(60, 60, 67);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library/src/theme-chalk/...</font>`里定义

# <font style="color:rgb(60, 60, 67);">三、demo 演示</font>
可以通过快捷方式 `Ctrl+Shift+N` 转到文件，输入自己所想找到的文件

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742711401973-e24bf7a4-4560-4b54-ab19-87f7d7069ebc.png)

根据前面的目录结构，我们已经知道了entry是样例文件，那么我们要找的文件，就是在 <font style="color:rgb(60, 60, 67);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">entry\src\main\ets\pages\base\TextEllipsis.ets</font>`里

```typescript
import router from '@ohos.router';
import { CONTAINER_SIZE, modeColor, SPACE } from '../../assets/styles/BaseStyle';
import { ComponentRouterParams } from '../../assets/global.type';
import ComponentShowContainer from '../../components/ComponentShowContainer';
import { IBestNavBar, IBestTextEllipsis } from '@ibestservices/ibest-ui';
@Entry
@Component
struct TextEllipsisPage {
    @State title: string = (router.getParams() as ComponentRouterParams).title || ''
    @State text: string = '人一生的大部分时间都是平淡无奇的。这也是我们身体养精蓄锐的必要条件。因为只有身心在泛起涟漪的生活中得到充分的修正，才能圆满的迎接人生的下一次高峰。'
    @State text1: string = "那一天我二十一岁，在我一生的黄金时代。我有好多奢望。我想爱，想吃，还想在一瞬间变成天上半明半暗的云。后来我才知道，生活就是个缓慢受锤的过程，人一天天老下去，奢望也一天天消失，最后变得像挨了锤的牛一样。可是我过二十一岁生日时没有预见到这一点。我觉得自己会永远生猛下去，什么也锤不了我。"

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
                    ComponentShowContainer({ title: '基础用法' }) {
                        IBestTextEllipsis({
                            text: this.text
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: '展开/收起' }) {
                        IBestTextEllipsis({
                            text: $r("app.string.app_desc"),
                            showAction: true
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: '自定义展示行数' }) {
                        Column({space: SPACE.SM}){
                            IBestTextEllipsis({
                                text: this.text1,
                                showAction: true,
                                rows: 3
                            })
                            Row(){
                                IBestTextEllipsis({
                                    text: this.text1,
                                    showAction: true,
                                    rows: 3
                                })
                            }.width(200)
                        }
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: '自定义省略位置' }){
                        ComponentShowContainer({ title: '头部省略' }) {
                            IBestTextEllipsis({
                                text: this.text1,
                                showAction: true,
                                omitPosition: 'start'
                            })
                        }
                        ComponentShowContainer({ title: '中部省略' }) {
                            IBestTextEllipsis({
                                text: this.text1,
                                omitPosition: 'middle',
                                showAction: true,
                                rows: 2
                            })
                        }
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: '自定义省略内容' }) {
                        IBestTextEllipsis({
                            text: this.text,
                            omitContent: "•••"
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: '自定义操作样式' }) {
                        IBestTextEllipsis({
                            text: this.text,
                            showAction: true,
                            expandText: "平铺",
                            collapseText: "折叠",
                            actionColor: "#DB3131"
                        })
                    }
                }
            }
            .layoutWeight(1)
            .padding({
                left: SPACE.SM,
                right: SPACE.SM
            })
        }
        .width(CONTAINER_SIZE.FULL)
        .height(CONTAINER_SIZE.FULL)
        .backgroundColor(modeColor.bg2)
    }
}
```

运行下demo，看下demo使用效果

![](https://cdn.nlark.com/yuque/0/2025/gif/26122809/1742713948933-8a6a5590-d1c7-42b8-b8c7-1be5943cc32a.gif)

# 四、源码解析
## <font style="color:rgb(60, 60, 67);">快速找到源代码位置</font>
可以通过快捷方式 `Ctrl+Shift+N` 转到文件，输入自己所想找到的文件

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742710974904-8c01e4be-a4c1-4768-af9a-685d539beac9.png)

根据前面的目录结构，我们已经知道了entry是样例文件，library是组件库文件，那么我们要找的文件，就是在 <font style="color:rgb(60, 60, 67);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\ets\components\textEllipsis</font>`里

进到文件里，我们可以看到有两个文件

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742714075224-707c8912-bd25-47aa-b34b-961f7d37baf6.png)

color.est文件定义了相关样式，可以看到，在 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\resources\base\element\color.json</font>` 读取样式，好处理全局样式

```javascript
interface IBestTextEllipsisColorType {
    textColor: ResourceColor
}

export const IBestTextEllipsisColor: IBestTextEllipsisColorType = {
    textColor: $r("app.color.ibest_text_color")
}
```

接下来就看下主文件<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">index.est</font>（已添加相关注释）

```javascript
/**
 * 导入必要的类型和工具函数，主要获取相关公共样式
 */
import { IBestStringNumber } from '../../model/Global.type'
import { getDefaultBaseStyle, IBEST_UI_NAMESPACE } from '../../theme-chalk/src'
import { CONTAINER_SIZE } from '../../theme-chalk/src/container'
import { IBestUIBaseStyleObjType } from '../../theme-chalk/src/index.type'
import {
  convertDimensionsWidthUnit,
  getComponentsInfo,
  getResourceStr,
  getSizeByUnit,
  measureTextSize
} from '../../utils/utils'
import { IBestTextEllipsisColor } from './color'

/**
 * 定义一个文本省略组件，支持多行省略、操作按钮等功能
 */
@Component
export struct IBestTextEllipsis {
  /**
   * 全局公共样式，从存储中加载默认主题样式
   */
  @StorageLink(IBEST_UI_NAMESPACE) baseStyle: IBestUIBaseStyleObjType = getDefaultBaseStyle()
  /**
   * 显示的文本内容
   */
  @Prop @Watch("formatText") text: ResourceStr = ""
  /**
   * 文字大小，默认为全局样式的中等字体大小
   */
  @Prop textFontSize: IBestStringNumber = this.baseStyle.fontSizeMd as string
  /**
   * 文字颜色，默认为组件定义的颜色
   */
  @Prop textColor: ResourceColor = IBestTextEllipsisColor.textColor
  /**
   * 行高，默认值为20px
   */
  @Prop lineHeight: IBestStringNumber = convertDimensionsWidthUnit(20)
  /**
   * 展示的行数，默认为1行
   */
  @Prop @Watch("formatText") rows: number = 1
  /**
   * 是否显示展开/收起操作按钮
   */
  @Prop showAction: boolean = false
  /**
   * 展开操作文案，默认为资源字符串
   */
  @Prop expandText: ResourceStr = $r("app.string.ibest_text_expand")
  /**
   * 收起操作文案，默认为资源字符串
   */
  @Prop collapseText: ResourceStr = $r("app.string.ibest_text_collapse")
  /**
   * 省略号内容，默认为“…”
   */
  @Prop omitContent: ResourceStr = "…"
  /**
   * 操作文字颜色，默认为全局样式的主色
   */
  @Prop actionColor: ResourceColor = this.baseStyle.primary
  /**
   * 省略位置，可选值为"start"、"middle"、"end"，默认为"end"
   */
  @Prop @Watch("formatText") omitPosition: "start" | "middle" | "end" = "end"
  /**
   * 组件状态变量
   */
  @State uniId: number = 0 // 唯一标识符
  @State showText: string = "" // 当前显示的文本
  @State isExpand: boolean = false // 是否展开
  @State textWidth: number = 0 // 文本宽度
  @State textHeight: number = 0 // 文本高度
  @State maxLineHeight: number = 0 // 最大行高
  /**
   * 私有属性：获取UI上下文
   */
  private uiContext = this.getUIContext()

  /**
   * 定义省略号内容的构建器
   */
  @Builder
  OmitContent() {
    Span(this.omitContent)
      .fontColor(this.textColor)
      .fontSize(getSizeByUnit(this.textFontSize, true))
  }

  /**
   * 组件即将显示时触发，初始化唯一ID并格式化文本
   */
  aboutToAppear(): void {
    this.uniId = this.getUniqueId()
    this.formatText()
  }

  /**
   * 获取文本字符串，从资源中解析文本内容
   * @returns 文本字符串
   */
  getTextString(){
    return getResourceStr(this.text)
  }

  /**
   * 获取省略号文本，从资源中解析省略号内容
   * @returns 省略号文本
   */
  getOmitText(){
    return getResourceStr(this.omitContent)
  }

  /**
   * 获取展开文本，从资源中解析展开操作文案
   * @returns 展开文本
   */
  getExpandText(){
    return getResourceStr(this.expandText)
  }

  /**
   * 格式化文本，根据容器宽度和行数计算显示文本
   */
  formatText(){
    setTimeout(() => {
      // 获取组件宽度、文本高度和最大行高
      this.textWidth = getComponentsInfo(this.uiContext, `ibest_text_${this.uniId}`).width
      this.textHeight = this.measureText(this.getTextString())
      this.maxLineHeight = this.measureText(this.getTextString(), this.rows)

      // 如果文本高度超过最大行高，则截取文本
      if (this.textHeight > this.maxLineHeight) {
        this.getTextByWidth()
      } else {
        this.showText = this.getTextString()
      }
    }, 0)
  }

  /**
   * 根据宽度截取文本，支持不同省略位置
   */
  getTextByWidth(){
    let clipText = this.getTextString()
    let textHeight = this.textHeight
    let centerIndex = Math.floor(clipText.length / 2)
    let leftStr = clipText.slice(0, centerIndex)
    let rightStr = clipText.slice(centerIndex)
    let omitText = this.getOmitText()
    let expandText = this.getExpandText()

    // 循环截取文本直到满足高度要求
    while (textHeight > this.maxLineHeight) {
      switch (this.omitPosition) {
        case "start":
          clipText = clipText.substring(1)
          textHeight = this.measureText(omitText + clipText + (this.showAction ? expandText : ""))
          break
        case "middle":
          leftStr = leftStr.substring(0, leftStr.length - 1)
          rightStr = rightStr.substring(1)
          textHeight = this.measureText(leftStr + omitText + rightStr + (this.showAction ? expandText : ""))
          break
        case "end":
          clipText = clipText.substring(0, clipText.length - 1)
          textHeight = this.measureText(clipText + (this.textHeight > this.maxLineHeight ? omitText : "") +
            (this.showAction ? expandText : ""))
          break
      }
    }

    // 设置最终显示文本
    this.showText = this.omitPosition == 'middle' ? leftStr + omitText + rightStr : clipText
  }

  /**
   * 测量文本高度
   * @param text 文本内容
   * @param rows 最大行数
   * @returns 文本高度
   */
  measureText(text: string, rows?: number): number {
    return measureTextSize(this.uiContext, {
      textContent: text,
      constraintWidth: this.textWidth,
      fontSize: getSizeByUnit(this.textFontSize, true),
      lineHeight: getSizeByUnit(this.lineHeight),
      maxLines: rows
    }).height
  }

  /**
   * 构建组件UI
   */
  build() {
    Text() {
      // 根据省略位置和是否展开，动态显示省略号和操作按钮
      if (this.textHeight > this.maxLineHeight && !this.isExpand && this.omitPosition == "start") {
        this.OmitContent()
      }
      Span(this.isExpand ? this.text : this.showText)
        .fontColor(this.textColor)
        .fontSize(getSizeByUnit(this.textFontSize, true))
      if (this.textHeight > this.maxLineHeight && !this.isExpand && this.omitPosition == "end") {
        this.OmitContent()
      }
      if (this.showAction) {
        Span(this.isExpand ? this.collapseText : this.expandText)
          .fontColor(this.actionColor)
          .fontSize(getSizeByUnit(this.textFontSize, true))
          .onClick(() => {
            this.isExpand = !this.isExpand
          })
      }
    }
    .width(CONTAINER_SIZE.FULL)
    .lineHeight(getSizeByUnit(this.lineHeight))
    .id(`ibest_text_${this.uniId}`)
    .visibility(this.showText ? Visibility.Visible : Visibility.Hidden)
  }
}
```

## 代码解释
### 1. 导入必要的工具和类型
首先，代码导入了一些外部工具和类型：

+ `IBestStringNumber`：用于定义字体大小等数值的类型。
+ `getDefaultBaseStyle` 和 `IBEST_UI_NAMESPACE`：从主题配置中加载全局样式。
+ `CONTAINER_SIZE`：定义容器宽度的常量。
+ 一些工具函数如 `convertDimensionsWidthUnit`、`getComponentsInfo` 等，用于处理单位转换、获取组件信息等。

这些工具和类型为后续的功能实现提供了基础支持，主要还是获取公共样式，方便做响应式

### 2. 定义组件结构
`IBestTextEllipsis` 是一个组件，它有以下几个主要部分：

#### (1) 属性（Props）
组件通过 `@Prop` 定义了一些属性，用户可以通过这些属性自定义组件的行为和外观：

+ `text`：要显示的文本内容。
+ `textFontSize`：文字大小，默认使用全局样式的中等字体大小。
+ `textColor`：文字颜色，默认是组件定义的颜色。
+ `lineHeight`：行高，默认值为 20px。
+ `rows`：展示的行数，默认为 1 行。
+ `showAction`：是否显示“展开/收起”按钮。
+ `expandText` 和 `collapseText`：分别是“展开”和“收起”的文案。
+ `omitContent`：省略号的内容，默认是“…”。
+ `actionColor`：操作按钮的文字颜色，默认是全局样式的主色。
+ `omitPosition`：省略位置，可选值为 "start"（开头省略）、"middle"（中间省略）、"end"（结尾省略），默认是 "end"。
+ ![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742715065597-1fc0cd98-977a-42f2-ac39-1b60e87235af.png)

#### (2) 状态（State）
组件通过 `@State` 定义了一些状态变量，用于保存组件运行时的数据：

+ `uniId`：唯一标识符，用于区分多个组件实例。
+ `showText`：当前显示的文本内容。
+ `isExpand`：是否处于展开状态。
+ `textWidth`、`textHeight`、`maxLineHeight`：分别表示文本宽度、高度以及最大允许的高度。

#### (3) 私有属性
`uiContext` 是一个私有属性，用于获取组件的 UI 上下文信息。

#### (4) 构建器
`OmitContent` 是一个构建器，用于生成省略号的显示内容，包括文字颜色和字体大小。

```javascript
  /**
   * 定义省略号内容的构建器
   */
  @Builder
  OmitContent() {
    Span(this.omitContent)
      .fontColor(this.textColor)
      .fontSize(getSizeByUnit(this.textFontSize, true))
  }
```

#### 3. 核心方法
组件实现了几个核心方法来处理文本省略和显示逻辑：

##### (1) `aboutToAppear`
当组件即将显示时，会调用这个方法：

+ 生成一个唯一的 `uniId`。
+ 调用 `formatText` 方法对文本进行格式化处理。

##### (2) `getResourceStr`
从资源中解析出文本内容，返回一个字符串。

:::info
可以学习下，封装组件的时候，方便使用中决策传_<font style="color:rgb(105, 61, 214);">ResourceStr</font>_，或是_<font style="color:rgb(105, 61, 214);">string</font>_

:::

```javascript
/**
 * 获取Resource字符串
 */
export function getResourceStr(res: ResourceStr): string {
	if(typeof res == 'string'){
		return res
	}
	return GlobalStore.context.resourceManager.getStringSync(res)
}
```

这段代码定义了一个名为 `getResourceStr` 的函数，用于获取资源字符串。其功能是根据传入的参数类型，返回对应的字符串值。具体逻辑如下：  

1. **参数说明**：  
    - `res: ResourceStr`：输入参数，可能是字符串类型或资源 ID 类型。
2. **逻辑判断**：  
    - 如果 `res` 是字符串类型（`typeof res == 'string'`），直接返回该字符串。  
    - 如果 `res` 不是字符串类型，则调用 `GlobalStore.context.resourceManager.getStringSync(res)` 方法，将资源 ID 转换为实际的字符串值并返回。
    - ![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742716376911-fbb8cf99-91af-4d80-81ba-92d8c6cf2be9.png)
3. **用途**：  
    - 该函数主要用于处理资源字符串的动态获取，支持直接传入字符串或资源 ID 的场景，增强了代码的灵活性和可维护性。

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742716301962-8112a4e1-211e-4468-9721-e81c657ad9c2.png)

##### (3) `getTextString`、 `getOmitText` 和 `getExpandText`
分别获取省略号文本和“展开”文案。

```javascript
  /**
   * 获取文本字符串，从资源中解析文本内容
   * @returns 文本字符串
   */
  getTextString(){
    return getResourceStr(this.text)
  }

  /**
   * 获取省略号文本，从资源中解析省略号内容
   * @returns 省略号文本
   */
  getOmitText(){
    return getResourceStr(this.omitContent)
  }

  /**
   * 获取展开文本，从资源中解析展开操作文案
   * @returns 展开文本
   */
  getExpandText(){
    return getResourceStr(this.expandText)
  }
```

##### (4) `formatText`
这是最关键的逻辑之一，用于根据容器宽度和行数计算显示的文本：

+ `setTimeout(()=>{},0)`让DOM更新后计算尺寸（这波叫异步防抖！）
+ 获取组件的宽度、文本高度以及最大允许的高度。
+ 如果文本高度超过最大高度，则调用 `getTextByWidth` 方法对文本进行截取。
+ 否则直接将原始文本赋值给 `showText`。

```javascript
  /**
   * 格式化文本，根据容器宽度和行数计算显示文本
   */
  formatText(){
    setTimeout(() => {
      // 获取组件宽度、文本高度和最大行高
      this.textWidth = getComponentsInfo(this.uiContext, `ibest_text_${this.uniId}`).width
      this.textHeight = this.measureText(this.getTextString())
      this.maxLineHeight = this.measureText(this.getTextString(), this.rows)

      // 如果文本高度超过最大行高，则截取文本
      if (this.textHeight > this.maxLineHeight) {
        this.getTextByWidth()
      } else {
        this.showText = this.getTextString()
      }
    }, 0)
  }
```

    - `getComponentsInfo`获取组件真实宽度（动态容器也能Hold住！）  

```javascript
/**
 * 获取组件信息
 * @param {context} UIContext
 * @param {key} 组件id
 * */
export const getComponentsInfo = (context: UIContext, key: string): ComInfoType => {
	let comUtils: ComponentUtils = context.getComponentUtils()
	let info: componentUtils.ComponentInfo = comUtils.getRectangleById(key)
	return {
		width: px2vp(info.size.width),
		height: px2vp(info.size.height),
		localLeft: px2vp(info.localOffset.x),
		localTop: px2vp(info.localOffset.y),
		screenLeft: px2vp(info.screenOffset.x),
		screenTop: px2vp(info.screenOffset.y),
		windowLeft: px2vp(info.windowOffset.x),
		windowTop: px2vp(info.windowOffset.y)
	}
}
```

##### (5) `getTextByWidth`
根据指定的省略位置（`omitPosition`）对文本进行截取：

+ 对于 "start"，从开头逐步删除字符。
+ 对于 "middle"，从中间逐步删除字符。
+ 对于 "end"，从结尾逐步删除字符。
+ 每次删除后重新测量文本高度，直到满足高度要求。

```javascript
  /**
   * 根据宽度截取文本，支持不同省略位置
   */
  getTextByWidth(){
    let clipText = this.getTextString()
    let textHeight = this.textHeight
    let centerIndex = Math.floor(clipText.length / 2)
    let leftStr = clipText.slice(0, centerIndex)
    let rightStr = clipText.slice(centerIndex)
    let omitText = this.getOmitText()
    let expandText = this.getExpandText()

    // 循环截取文本直到满足高度要求
    while (textHeight > this.maxLineHeight) {
      switch (this.omitPosition) {
        case "start":
          clipText = clipText.substring(1)
          textHeight = this.measureText(omitText + clipText + (this.showAction ? expandText : ""))
          break
        case "middle":
          leftStr = leftStr.substring(0, leftStr.length - 1)
          rightStr = rightStr.substring(1)
          textHeight = this.measureText(leftStr + omitText + rightStr + (this.showAction ? expandText : ""))
          break
        case "end":
          clipText = clipText.substring(0, clipText.length - 1)
          textHeight = this.measureText(clipText + (this.textHeight > this.maxLineHeight ? omitText : "") +
            (this.showAction ? expandText : ""))
          break
      }
    }

    // 设置最终显示文本
    this.showText = this.omitPosition == 'middle' ? leftStr + omitText + rightStr : clipText
  }
```

##### (6) `measureText`
测量文本的高度，传入参数包括文本内容、字体大小、行高等信息。

```javascript
  /**
   * 测量文本高度
   * @param text 文本内容
   * @param rows 最大行数
   * @returns 文本高度
   */
  measureText(text: string, rows?: number): number {
    return measureTextSize(this.uiContext, {
      textContent: text,
      constraintWidth: this.textWidth,
      fontSize: getSizeByUnit(this.textFontSize, true),
      lineHeight: getSizeByUnit(this.lineHeight),
      maxLines: rows
    }).height
  }
```

#### 4. 构建 UI
`build` 方法定义了组件的 UI 结构：

+ 如果文本高度超过最大高度且未展开，会在开头或结尾显示省略号。
+ 显示当前的文本内容（`showText` 或完整的 `text`）。
+ 如果启用了操作按钮（`showAction`），会显示“展开”或“收起”按钮，点击后切换展开状态。

#### 5.控制流图
![](https://cdn.nlark.com/yuque/__mermaid_v3/9bc4b47245c3f4668565713b602beb53.svg)

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742715258924-7578896f-b58b-433c-8325-ada46fd68ca4.png)

##### 流程图说明：
1. **初始化组件**：加载全局样式、设置默认属性值。
2. **判断文本是否超出高度**：通过 `measureText` 方法测量文本高度，与最大允许高度比较。
3. **截取文本**：如果超出高度，调用 `getTextByWidth` 方法，根据省略位置（`start`、`middle`、`end`）逐步截取文本。
4. **测量截取后文本的高度**：每次截取后重新测量高度，直到满足高度要求。
5. **设置最终显示文本**：将截取后的文本赋值给 `showText`。
6. **构建UI**：根据 `showText` 和其他属性动态生成组件的 UI。

# 五. 总结
这次拆的是IBest-UI的TextEllipsis组件，这个组件的核心功能是：

1. 根据容器宽度和行数自动截取文本，并支持不同的省略位置（开头、中间、结尾）。
2. 提供“展开/收起”按钮，方便用户查看完整内容。
3. 支持自定义字体大小、颜色、行高等样式。

通过这些功能，开发者可以轻松实现多行文本的省略效果，并提供良好的用户体验。

HarmonyOS NEXT开发想搞定长文本显示？这个组件直接抄作业！

#### 
