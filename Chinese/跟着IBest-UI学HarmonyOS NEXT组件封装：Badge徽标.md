### **一、引言：一起来啃源码，解锁HarmonyOS NEXT的“组件密码”！  **
嘿，小伙伴们！今天想和大家聊一个超实用的开源项目——**IBest-UI**，一个专为鸿蒙生态打造的轻量级UI组件库。如果你正在开发HarmonyOS NEXT应用，一定遇到过这些痛点：重复造轮子、适配多端界面费时费力、深浅模式切换麻烦……别急，IBest-UI就是来“救场”的！  

**它有多香？**  

+ **轻量到飞起**：核心代码精简，引入即用，绝不给你添负担。  
+ **主题随心换**：深色模式？浅色模式？一行代码切换，适配鸿蒙元服务毫无压力。  
+ **功能小而美**：从按钮到弹窗，从徽章到导航栏，覆盖高频场景，样式参考vant，使用过vant的，都知道vant样式有多好看！

但今天咱们不光是“用组件”，而是要**打开引擎盖，看看里面的“黑科技”**！我们发起一个**源码共读计划**，目标很简单：  

1. **拆解设计思想**：比如Badge徽标组件，它是怎么实现动态更新、如何优雅适配不同设备？  
2. **偷师IBest-UI**：在源码中捕捉ArkTS的高阶用法，<font style="color:rgb(37, 41, 51);">学习如何在HarmonyOS NEXT中用声明式UI开发“丝滑”应用。</font>
3. **边学边玩**：欢迎随时抛出问题、提交PR，咱们一起让IBest-UI变得更强大！

无论你是想提升源码阅读能力，还是想摸透鸿蒙开发的门道，这个系列都会是你的“实战指南”。准备好和我一起挖宝了吗？Let’s go！ 🚀  

### 二、准备工作
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

### <font style="color:rgb(60, 60, 67);">三、快速找到源代码位置</font>
可以通过快捷方式 `Ctrl+Shift+N` 转到文件，输入自己所想找到的文件

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742397422296-87dafcbf-1727-4437-8dbd-bdcdc3d2de1b.png)

根据前面的目录结构，我们已经知道了entry是样例文件，library是组件库文件，那么我们要找的文件，就是在 <font style="color:rgb(60, 60, 67);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\ets\components\badge</font>`里

### 四、源码解析
进到文件里，我们可以看到有三个文件

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742398817117-db21cf96-f4c0-47f4-877a-787a9a341c67.png)

color.est文件定义了相关样式，可以看到，在 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\resources\base\element\color.json</font>` 读取样式，好处理全局样式

```javascript
interface IBestBadgeColorType {
    badgeBgColor: ResourceColor
    textColor: ResourceColor
}

export const IBestBadgeColor: IBestBadgeColorType = {
    badgeBgColor: $r("app.color.ibest_badge_background"),
    textColor: $r("app.color.ibest_badge_text_color")
}
```

<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">index.type.ets</font>文件定义了<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">IBestBadgePosition</font>的类型

```javascript
/**
 * 徽标位置
 */
export type IBestBadgePosition = "top-left" | "top-right" | "bottom-left" | "bottom-right"
```

接下来就看下主文件<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">index.est</font>

```javascript
// 获取全局样式
import { getDefaultBaseStyle, IBEST_UI_NAMESPACE } from "../../theme-chalk/src"
import { IBestUIBaseStyleObjType } from "../../theme-chalk/src/index.type"
// 根据单位转换尺寸 用于框架固定尺寸格式化 带单位
import { convertDimensionsWidthUnit } from "../../utils/utils"
// 获取样式
import { IBestBadgeColor } from "./color"
import { IBestBadgePosition } from "./index.type"

/**
 * IBestBadge组件用于展示徽标，可以显示徽标内容、背景色、位置等
 */
@Component
export struct IBestBadge {
	/**
	 * 全局公共样式
	 */
	@StorageLink(IBEST_UI_NAMESPACE) baseStyle: IBestUIBaseStyleObjType = getDefaultBaseStyle()
	/**
	 * 徽标内容
	 */
	@Prop content: string | number = ''
	/**
	 * 徽标背景色
	 */
	@Prop color: ResourceColor = IBestBadgeColor.badgeBgColor
	/**
	 * 是否展示为小红点
	 */
	@Prop dot: boolean = false
	/**
	 * 最大值,超过最大值会显示 {max}+,仅当 content 为数字时有效
	 */
	@Prop max: number = -1
	/**
	 * 值为0时是否显示徽标
	 */
	@Prop showZero: boolean = true
	/**
	 * 徽标位置
	 */
	@Prop badgePosition: IBestBadgePosition = 'top-right'
	/**
	 * 自定义内容
	 */
	@BuilderParam defaultBuilder?: CustomBuilder

	/**
	 * 判断徽标是否显示
	 * @returns boolean 表示徽标是否应该显示
	 */
	isShow(){
		return !(typeof this.content == "number" && this.content == 0 && !this.showZero)
	}

	/**
	 * 获取徽标内容
	 * @returns string 表示要显示的徽标内容
	 */
	getContent(){
		if(typeof this.content == 'number' && this.max > 0 && this.content > this.max){
			return this.max + '+'
		}
		return this.content.toString()
	}

	/**
	 * 根据徽标位置获取边缘位置
	 * @returns Edges 表示徽标的边缘位置
	 */
	getPosition(): Edges{
		switch (this.badgePosition){
			case 'top-left':
				return {
					left: 0,
					top: 0
				}
			case 'top-right':
				return {
					right: 0,
					top: 0
				}
			case 'bottom-left':
				return {
					left: 0,
					bottom: 0
				}
			case 'bottom-right':
				return {
					right: 0,
					bottom: 0
				}
		}
	}

	/**
	 * 根据徽标位置获取平移选项
	 * @returns TranslateOptions 表示徽标的平移选项
	 */
	getTranslate(): TranslateOptions{
		switch (this.badgePosition){
			case 'top-left':
				return {
					x: "-50%",
					y: "-50%"
				}
			case 'top-right':
				return {
					x: "50%",
					y: "-50%"
				}
			case 'bottom-left':
				return {
					x: "-50%",
					y: "50%"
				}
			case 'bottom-right':
				return {
					x: "50%",
					y: "50%"
				}
		}
	}

	/**
	 * 构建徽标组件
	 */
	build() {
		Row() {
			if (this.defaultBuilder) {
				this.defaultBuilder()
			}
			if (this.dot) {
				Text()
					.width(convertDimensionsWidthUnit(8))
					.aspectRatio(1)
					.borderRadius(this.baseStyle.borderRadiusMax)
					.backgroundColor(this.color)
					.position(this.getPosition())
					.translate(this.getTranslate())
			} else if(this.isShow()) {
				Text(this.getContent())
					.constraintSize({ minWidth: convertDimensionsWidthUnit(16) })
					.fontColor(IBestBadgeColor.textColor)
					.fontSize(convertDimensionsWidthUnit(12, true))
					.padding({ left: convertDimensionsWidthUnit(3), right: convertDimensionsWidthUnit(3) })
					.backgroundColor(this.color)
					.borderRadius(this.baseStyle.borderRadiusMax)
					.position(this.getPosition())
					.translate(this.getTranslate())
					.textAlign(TextAlign.Center)
			}
		}
	}
}
```

#### 代码解释
这段代码实现了一个名为 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">IBestBadge</font>` 的徽标组件，主要用于展示带有内容、背景色和位置的徽标。以下是详细功能分解：

1. **属性定义**：
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">baseStyle</font>`：全局公共样式，通过 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">@StorageLink</font>` 绑定。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">cotnten</font>`：徽标内容，可以是字符串或数字。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">color</font>`：徽标背景色，默认为 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">IBestBadgeColor.badgeBgColor</font>`。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">dot</font>`：是否显示为小红点。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">max</font>`：徽标内容的最大值，超过时显示 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">{max}+</font>`。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">showZero</font>`：当 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">content</font>` 为 0 时是否显示徽标。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">badgePosition</font>`：徽标位置，支持 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">top-left</font>`、`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">top-right</font>`、`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">bottom-left</font>` 和 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">bottom-right</font>`。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">defaultBuilder</font>`：自定义内容的构建器。
+ ![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742576249855-63fe331b-5d41-419d-9824-4c570b29db1b.png)
+ <font style="color:rgb(89, 89, 89);">定义了一系列 </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">props</font>`<font style="color:rgb(89, 89, 89);">，包括徽标内容，徽标背景色、是否展示为小红点、徽标位置等。可直接参见文档中的</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">API</font>`<font style="color:rgb(89, 89, 89);">属性。</font>
+ <font style="color:rgb(89, 89, 89);"></font>
+ **方法逻辑**：
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">isShow</font>`：判断徽标是否需要显示，当 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">content</font>` 为 0 且 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">showZero</font>` 为 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">false</font>` 时不显示。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">getContent</font>`：根据 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">max</font>` 值限制返回徽标内容，若超出最大值则显示 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">{max}+</font>`。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">getPosition</font>`：根据 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">badgePosition</font>` 返回徽标的边缘位置（如 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">top-left</font>` 对应 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">{left: 0, top: 0}</font>`）。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">getTranslate</font>`：根据 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">badgePosition</font>` 返回徽标的平移选项（如 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">top-left</font>` 对应 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">{x: "-50%", y: "-50%"}</font>`）。
    - `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">build</font>`：构建徽标组件，优先渲染 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">defaultBuilder</font>` 内容；若为小红点模式，则创建小红点；否则根据 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">isShow</font>` 和 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">getContent</font>` 渲染普通徽标。
6. **渲染逻辑**：
    - 若 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">dot</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);"> </font>为 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">true</font>`，渲染一个小红点。
    - 若 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">isShow</font>` 返回 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">true</font>`，渲染普通徽标，并应用内容、样式和位置。

---

#### 控制流图
![](https://cdn.nlark.com/yuque/__mermaid_v3/950d362b6a14b53ced5420c6c4dba653.svg)

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1743089172562-4c45b6b4-8cdf-4691-ba4e-e609751587d0.png)



这个Badge组件，源码很简单，不复杂（相关代码解释已在源码文件里写好了），为什么解析这么简单的组件，主要是想着由简入深，先熟悉下HarmonyOS NEXT的ArkTs的写法。

### **五、**<font style="color:rgb(60, 60, 67);">自定义主题样式</font>
其中，我们看组件的第一个属性`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">baseStyle</font>`：全局公共样式，通过 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">@StorageLink</font>` 绑定。

@StorageLink，<font style="color:rgba(0, 0, 0, 0.9);">从API version 11开始，该装饰器支持在元服务中使用。在harmonyOS NEXT中，可以适用。</font>

<font style="color:rgba(0, 0, 0, 0.9);">让我们看下官方文档的说明</font>

:::info
<font style="color:rgba(0, 0, 0, 0.9);">@StorageLink(key)是和AppStorage中key对应的属性建立双向数据同步：</font>

1. <font style="color:rgb(36, 39, 40);">本地修改发生，该修改会被写回AppStorage中。</font>
2. <font style="color:rgb(36, 39, 40);">AppStorage中的修改发生后，该修改会被同步到所有绑定AppStorage对应key的属性上，包括单向（@StorageProp和通过Prop创建的单向绑定变量）、双向（@StorageLink和通过Link创建的双向绑定变量）变量和其他实例（比如PersistentStorage）。</font>

:::

 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">@StorageLink</font>`要配合着<font style="color:rgb(36, 39, 40);"> </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">AppStorage</font>`<font style="color:rgb(36, 39, 40);">使用，让我们看看</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">AppStorage</font>`<font style="color:rgb(36, 39, 40);">怎么进行初始化</font>

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742653396620-26fb60cc-4f06-4d07-ae17-7066b4beed4c.png)

已知初始化方法为`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">AppStorage.setOrCreate(propName, newValue)</font>`<font style="color:rgb(36, 39, 40);">，那我们找找看，该方法在哪初始化，可以通过快捷方式 </font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">Ctrl+Shift+N</font>`，通过Text，来快速找到初始化的文件  
 ![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742653596923-3e59b441-f78a-4f06-8d08-e34c1d40b18b.png)

进到文件`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\ets\theme-chalk\src\index.ets</font>`<font style="color:rgb(36, 39, 40);">，我们可以找到样式初始化的方法</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">setIBestUIBaseStyle</font>

通过<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">setIBestUIBaseStyle</font><font style="color:rgb(36, 39, 40);">方法，设置全局样式。</font>

```typescript
/**
 * AppStorage命名空间
 */
export const IBEST_UI_NAMESPACE = '__IBEST-UI_BASE_STYLE'
/**
 * 设置全局样式
 * @param styleData
 */
export function setIBestUIBaseStyle(styleData?: Partial<IBestUIBaseStyleType>) {
	const newStyleData = getDefaultBaseStyle();
	if (typeof styleData === 'object' && styleData !== null) {
		Object.keys(styleData).forEach(item => {
			if (newStyleData[item]) {
				newStyleData[item] = (styleData as IBestUIBaseStyleObjType)[item] ?? newStyleData[item]
			}
		})
	}
	AppStorage.setOrCreate(IBEST_UI_NAMESPACE, newStyleData)
}
```

1. `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">IBEST_UI_NAMESPACE</font>`** 常量**  
    - 定义了一个全局命名空间常量，用于标识存储的全局样式数据。
2. `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">setIBestUIBaseStyle</font>`** 方法**  
    - 该方法用于设置全局样式，接收一个可选参数 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">styleData</font>`，类型为 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">Partial<IBestUIBaseStyleType></font>`
    - 如果传入了有效的 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">styleData</font>` 对象，则将其与默认样式数据合并，覆盖默认值。
    - 最终将合并后的样式数据存储到 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">AppStorage</font>` 中，使用命名空间 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">__IBEST-UI_BASE_STYLE</font>`。

![](https://cdn.nlark.com/yuque/__mermaid_v3/c27bb3935c1f94c5425bc3688a0f76ea.svg)

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742658930653-80a4905d-cc26-4f04-b978-e1981395b830.png)

1. `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">getDefaultBaseStyle</font>`** 方法**  
    - 该方法用于生成框架默认的主题样式数据，返回一个 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">IBestUIBaseStyleObjType</font>` 类型的对象。
    - 数据包括颜色（如主题色、透明度）、间距（如 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">spaceMini</font>`、`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">spaceBase</font>`）、字体大小、边框半径、行高等多种样式属性。
    - 部分值通过 `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">convertDimensionsWidthUnit</font>` 方法进行单位转换。

```typescript
/**
 * 框架默认主题
 */
export function getDefaultBaseStyle(): IBestUIBaseStyleObjType {
	const data: IBestUIBaseStyleType = {
		default: THEME_COLOR.DEFAULT,
		primary: THEME_COLOR.PRIMARY,
		success: THEME_COLOR.SUCCESS,
		warning: THEME_COLOR.WARNING,
		danger: THEME_COLOR.DANGER,
		primaryOpacity: COLOR_OPACITY.PRIMARY,
		successOpacity: COLOR_OPACITY.SUCCESS,
		warningOpacity: COLOR_OPACITY.WARNING,
		dangerOpacity: COLOR_OPACITY.DANGER,
		spaceMini: convertDimensionsWidthUnit(SPACE.MINI),
		spaceBase: convertDimensionsWidthUnit(SPACE.BASE),
		spaceXs: convertDimensionsWidthUnit(SPACE.XS),
		spaceSm: convertDimensionsWidthUnit(SPACE.SM),
		spaceMd: convertDimensionsWidthUnit(SPACE.MD),
		spaceLg: convertDimensionsWidthUnit(SPACE.LG),
		spaceXl: convertDimensionsWidthUnit(SPACE.XL),
		fontSizeXs: convertDimensionsWidthUnit(FONT_SIZE.XS, true),
		fontSizeSm: convertDimensionsWidthUnit(FONT_SIZE.SM, true),
		fontSizeMd: convertDimensionsWidthUnit(FONT_SIZE.MD, true),
		fontSizeLg: convertDimensionsWidthUnit(FONT_SIZE.LG, true),
		fontSizeXl: convertDimensionsWidthUnit(FONT_SIZE.XL, true),
		borderRadiusSm: convertDimensionsWidthUnit(BORDER_RADIUS.SM),
		borderRadiusMd: convertDimensionsWidthUnit(BORDER_RADIUS.MD),
		borderRadiusLg: convertDimensionsWidthUnit(BORDER_RADIUS.LG),
		borderRadiusMax: convertDimensionsWidthUnit(BORDER_RADIUS.MAX),
		lineHeightXs: convertDimensionsWidthUnit(LINE_HEIGHT.XS),
		lineHeightSm: convertDimensionsWidthUnit(LINE_HEIGHT.SM),
		lineHeightMd: convertDimensionsWidthUnit(LINE_HEIGHT.MD),
		lineHeightLg: convertDimensionsWidthUnit(LINE_HEIGHT.LG),
		// 滚动效果
		scrollEdgeEffect: EdgeEffect.Fade,
		// 滚动条颜色
		scrollBarColor: '#dbdfe6',
		animationDuration: 200
	}
	return data;
}
```

<font style="color:rgb(36, 39, 40);">再让我们找找</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">setIBestUIBaseStyle</font><font style="color:rgb(36, 39, 40);">在那边调用，然后可以发现在</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\Index.ets</font>`<font style="color:rgb(36, 39, 40);">中将 </font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">setIBestUIBaseStyle</font><font style="color:rgb(36, 39, 40);"> 方法重命名为</font><font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);"> IBestSetUIBaseStyle</font><font style="color:rgb(36, 39, 40);">，并将此方法暴露出去。</font>

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742655578632-9ec781bf-701e-4140-881a-164a8f49758f.png)

在[IBest-UI@HarmonyOS-自定义主题样式](https://ibestui.ibestservices.com/guide/custom-theme/)文档中，可以看到暴露出来的<font style="color:rgb(36, 39, 40);">IBestSetUIBaseStyle方法</font>![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742656282287-0644aa0a-232e-49f0-ba35-ca396e904ddc.png)

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742656471424-8b4e041c-ffdc-417a-a439-ffdc3c492b23.png)

由小见大，我们知道了，IBest-UI@HarmonyOS组件库，是怎么实现自定义主题样式

### **六、结语**
#### <font style="color:rgb(44, 44, 54);">咱们一起盘盘这个HarmonyOS组件库！</font>
<font style="color:rgb(44, 44, 54);">这篇文章带大家拆解了IBest-UI组件库里的Badge徽标组件，顺便摸清了鸿蒙主题定制的套路。简单说就是：</font>**组件虽小，五脏俱全 **<font style="color:rgb(44, 44, 54);">，特别适合新手练手！</font>

#### <font style="color:rgb(44, 44, 54);">干了啥？</font>
1. **组件怎么用？**
    - <font style="color:rgb(44, 44, 54);">能显示数字/文字、变红点、自动截断超长内容（比如"99+"）</font>
    - <font style="color:rgb(44, 44, 54);">支持四个方位贴牌（左上右上随便钉！）</font>
    - **重点 **<font style="color:rgb(44, 44, 54);">：通过</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">@StorageLink</font>`<font style="color:rgb(44, 44, 54);">同步全局主题，换皮肤只需改配置文件，不用挨个改组件</font>
2. **源码里藏着啥宝贝？**
    - **属性全家桶 **<font style="color:rgb(44, 44, 54);">：</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">content</font>`<font style="color:rgb(44, 44, 54);">（内容）、</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">dot</font>`<font style="color:rgb(44, 44, 54);">（小红点开关）、</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">max</font>`<font style="color:rgb(44, 44, 54);">（最大值限制）... 想改啥直接传参</font>
    - **位置 **<font style="color:rgb(44, 44, 54);">：用</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">getPosition()</font>`<font style="color:rgb(44, 44, 54);">和</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">getTranslate()</font>`<font style="color:rgb(44, 44, 54);">控制徽标位置，自动计算偏移量（再也不用硬编码坐标啦！）</font>
    - **条件渲染 **<font style="color:rgb(44, 44, 54);">：</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">isShow()</font>`<font style="color:rgb(44, 44, 54);">判断要不要显示徽标，0值显示开关超贴心</font>
3. **主题定制怎么玩？**
    - <font style="color:rgb(44, 44, 54);">通过</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">setIBestUIBaseStyle</font>`<font style="color:rgb(44, 44, 54);">统一管理样式变量（颜色/圆角/间距...）</font>
    - <font style="color:rgb(44, 44, 54);">源码里直接</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">AppStorage</font>`<font style="color:rgb(44, 44, 54);">存全局配置，改一次全组件生效（妈妈再也不用担心我改漏文件！）</font>

#### <font style="color:rgb(44, 44, 54);">吐槽&彩蛋</font>
+ **ArkTS骚操作 **<font style="color:rgb(44, 44, 54);">：</font>`<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">convertDimensionsWidthUnit</font>`<font style="color:rgb(44, 44, 54);">这个工具函数，自动适配不同设备的尺寸单位，鸿蒙生态的"响应式"精髓就在这！</font>
+ **隐藏关卡 **<font style="color:rgb(44, 44, 54);">：组件库里还有按钮、导航栏等一堆组件，Badge只是开胃菜，后面可以继续挖坑！</font>

**一句话总结 **<font style="color:rgb(44, 44, 54);">：这组件库把鸿蒙的声明式UI玩明白了，跟着抄作业能少写不少代码，建议收藏！</font>

