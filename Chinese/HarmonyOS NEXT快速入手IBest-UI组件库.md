## <font style="color:rgb(60, 60, 67);">一、介绍</font>
<font style="color:rgb(60, 60, 67);">IBest-UI是一个</font>**<font style="color:rgb(60, 60, 67);">轻量、简单易用、可定制主题、支持深色模式和浅色模式</font>**<font style="color:rgb(60, 60, 67);">的鸿蒙开源UI组件库, 完美兼容元服务。是一个轻量、可定制的 HarmonyOS 组件库</font>

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1741966864471-aef6aba6-0b52-4166-a642-47e73da02ce3.png)

## 二、HarmonyOS Next开发环境准备
1. **开发工具配置**
+ 下载安装[DevEco Studio](https://developer.huawei.com/consumer/cn/deveco-studio/)
+ 配置SDK：Tools > SDK Manager > 勾选HarmonyOS Next 5.0.0(API 12)及以上版本
+ 环境验证：新建Empty Ability项目，选择"HarmonyOS"模板
+ ![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1741967476155-89b0d10a-b36b-4d01-8680-89ed303f0e91.png)

## 二、安装依赖
### 1. 安装依赖
```typescript
ohpm install @ibestservices/ibest-ui
```

### 2. <font style="color:rgb(60, 60, 67);">初始化</font>
在\项目\entry\src\main\ets\entryability\EntryAbility.ets 里初始化组件库

```typescript
import { IBestInit } from "@ibestservices/ibest-ui"

onWindowStageCreate(windowStage: window.WindowStage): void {
  windowStage.loadContent('pages/Index', (err, data) => {
    // 在此处初始化组件库!!!
    // 在此处初始化组件库!!!
    // 在此处初始化组件库!!!
    IBestInit(windowStage, this.context)
  })
}
```

### 3. <font style="color:rgb(60, 60, 67);">使用</font>
引入您想使用的组件，并使用

```typescript
// 引入您所想使用的组件
import { IBestButton } from "@ibestservices/ibest-ui"

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';

  build() {
    Column() {
      IBestButton({
        text: "主要按钮",
        type: 'primary'
      })
      IBestButton({
        text:  "成功按钮",
        type: 'success'
      })
      IBestButton({
        text:  "默认按钮",
        type: 'default'
      })
      IBestButton({
        text:  "危险按钮",
        type: 'danger'
      })
      IBestButton({
        text:  "警告按钮",
        type: 'warning'
      })
    }
    .height('100%')
    .width('100%')
  }
}
```

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1741968559657-ee6915f0-3cf4-40b3-9a88-55cea9336fe0.png)

## 三、进阶
### 1.<font style="color:rgb(60, 60, 67);">全局配置</font>
<font style="color:rgb(60, 60, 67);">组件库提供全局配置方法，供开发者根据自己项目实际情况选择合适的尺寸单位。</font>

#### <font style="color:rgb(60, 60, 67);">使用方法</font>
:::info
<font style="color:rgb(60, 60, 67);">请在初始化组件库之后调用!!!</font>

:::

\项目\entry\src\main\ets\entryability\EntryAbility.ets 

```plain
import { IBestSetGlobalConfig } from "@ibestservices/ibest-ui"

onWindowStageCreate(windowStage: window.WindowStage): void {
	windowStage.loadContent('pages/Index', (err, data) => {
		// 在此处初始化组件库!!!
		IBestInit(windowStage, this.context)
		// 请在初始化组件库之后调用!!!
		IBestSetGlobalConfig({
			unit: "lpx",
			fontUnit: "lpx"
		})
	})
}
```

#### <font style="color:rgb(60, 60, 67);">IBestGlobalConfigType 数据类型</font>
<font style="color:rgb(60, 60, 67);">该类型为 IBestSetGlobalConfig 方法的参数类型, 后续还会加入更多属性配置.</font>

| **参数** | **说明** | **类型** | **默认值** |
| --- | --- | --- | --- |
| <font style="color:rgb(60, 60, 67);">designWidth</font> | <font style="color:rgb(60, 60, 67);">标识页面设计基准宽度</font> | <font style="color:rgb(60, 60, 67);">number</font> | `720` |
| <font style="color:rgb(60, 60, 67);">unit</font> | <font style="color:rgb(60, 60, 67);">全局尺寸单位, 可选值</font><font style="color:rgb(60, 60, 67);"> </font>`vp`<br/><font style="color:rgb(60, 60, 67);">、</font>`px`<br/><font style="color:rgb(60, 60, 67);">、</font>`lpx` | <font style="color:rgb(60, 60, 67);">string</font> | `vp` |
| <font style="color:rgb(60, 60, 67);">fontUnit</font> | <font style="color:rgb(60, 60, 67);">全局字体单位, 可选值</font><font style="color:rgb(60, 60, 67);"> </font>`vp`<br/><font style="color:rgb(60, 60, 67);">、</font>`px`<br/><font style="color:rgb(60, 60, 67);">、</font>`lpx`<br/><font style="color:rgb(60, 60, 67);">、</font>`fp` | <font style="color:rgb(60, 60, 67);">string</font> | `fp` |


### <font style="color:rgb(60, 60, 67);">2.自定义主题样式</font>
<font style="color:rgb(60, 60, 67);">在需要更改主题样式时，可通过调用</font><font style="color:rgb(60, 60, 67);"> </font>`IBestSetUIBaseStyle`<font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">方法设置样式。</font>

#### <font style="color:rgb(60, 60, 67);">更改主题</font>
<font style="color:rgb(60, 60, 67);">• 在项目的启动页面中，通过调用 </font>`IBestSetUIBaseStyle`<font style="color:rgb(60, 60, 67);"> 方法，设置主题。  
</font><font style="color:rgb(60, 60, 67);">• 若项目使用了 </font>`IBestSetGlobalConfig`<font style="color:rgb(60, 60, 67);"> 方法, 请确保在该方法之后调用 </font>`IBestSetUIBaseStyle`<font style="color:rgb(60, 60, 67);"> !!!</font>

```plain
import { IBestSetUIBaseStyle } from "@ibestservices/ibest-ui";

onWindowStageCreate(windowStage: window.WindowStage): void {
	windowStage.loadContent('pages/Index', (err, data) => {
		IBestSetUIBaseStyle({
			primary: '#7232dd'
		})
	})
}
```

#### <font style="color:rgb(60, 60, 67);">IBestUIBaseStyleObjType 类型说明</font>
<font style="color:rgb(60, 60, 67);">• 该类型即为</font>`IBestSetUIBaseStyle`<font style="color:rgb(60, 60, 67);">方法的参数类型，均为非必填类型，传入值会覆盖默认值，暂时支持这么多预设样式，随着组件丰富逐步完善！  
</font><font style="color:rgb(60, 60, 67);">• 默认单位为</font><font style="color:rgb(60, 60, 67);"> </font>`vp`<font style="color:rgb(60, 60, 67);">, 当全局单位为</font><font style="color:rgb(60, 60, 67);"> </font>`lpx`<font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">时, 以下数值会依据</font><font style="color:rgb(60, 60, 67);"> </font>`designWidth`<font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">和当前设备屏幕宽度自动计算转化为lpx对应的视窗尺寸。</font>

| **参数** | **说明** | **类型** | **默认** |
| --- | --- | --- | --- |
| <font style="color:rgb(60, 60, 67);">primary</font> | <font style="color:rgb(60, 60, 67);">primary 反馈色</font> | <font style="color:rgb(60, 60, 67);">string</font> | `#3D8AF2` |
| <font style="color:rgb(60, 60, 67);">success</font> | <font style="color:rgb(60, 60, 67);">success 反馈色</font> | <font style="color:rgb(60, 60, 67);">string</font> | `#58DB6B` |
| <font style="color:rgb(60, 60, 67);">warning</font> | <font style="color:rgb(60, 60, 67);">warning 反馈色</font> | <font style="color:rgb(60, 60, 67);">string</font> | `#F29C73` |
| <font style="color:rgb(60, 60, 67);">danger</font> | <font style="color:rgb(60, 60, 67);">danger 反馈色</font> | <font style="color:rgb(60, 60, 67);">string</font> | `#DB3131` |
| <font style="color:rgb(60, 60, 67);">default</font> | <font style="color:rgb(60, 60, 67);">default 默认色</font> | <font style="color:rgb(60, 60, 67);">string</font> | `#FFFFFF` |
| <font style="color:rgb(60, 60, 67);">spaceMini</font> | <font style="color:rgb(60, 60, 67);">间距，一般用于</font><font style="color:rgb(60, 60, 67);"> </font>`padding`<br/><font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">与</font><font style="color:rgb(60, 60, 67);"> </font>`margin` | <font style="color:rgb(60, 60, 67);">string</font> | `2` |
| <font style="color:rgb(60, 60, 67);">spaceBase</font> | <font style="color:rgb(60, 60, 67);">间距，一般用于</font><font style="color:rgb(60, 60, 67);"> </font>`padding`<br/><font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">与</font><font style="color:rgb(60, 60, 67);"> </font>`margin` | <font style="color:rgb(60, 60, 67);">string</font> | `4` |
| <font style="color:rgb(60, 60, 67);">spaceXs</font> | <font style="color:rgb(60, 60, 67);">间距，一般用于</font><font style="color:rgb(60, 60, 67);"> </font>`padding`<br/><font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">与</font><font style="color:rgb(60, 60, 67);"> </font>`margin` | <font style="color:rgb(60, 60, 67);">string</font> | `8` |
| <font style="color:rgb(60, 60, 67);">spaceSm</font> | <font style="color:rgb(60, 60, 67);">间距，一般用于</font><font style="color:rgb(60, 60, 67);"> </font>`padding`<br/><font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">与</font><font style="color:rgb(60, 60, 67);"> </font>`margin` | <font style="color:rgb(60, 60, 67);">string</font> | `12` |
| <font style="color:rgb(60, 60, 67);">spaceMd</font> | <font style="color:rgb(60, 60, 67);">间距，一般用于</font><font style="color:rgb(60, 60, 67);"> </font>`padding`<br/><font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">与</font><font style="color:rgb(60, 60, 67);"> </font>`margin` | <font style="color:rgb(60, 60, 67);">string</font> | `16` |
| <font style="color:rgb(60, 60, 67);">spaceLg</font> | <font style="color:rgb(60, 60, 67);">间距，一般用于</font><font style="color:rgb(60, 60, 67);"> </font>`padding`<br/><font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">与</font><font style="color:rgb(60, 60, 67);"> </font>`margin` | <font style="color:rgb(60, 60, 67);">string</font> | `24` |
| <font style="color:rgb(60, 60, 67);">spaceXl</font> | <font style="color:rgb(60, 60, 67);">间距，一般用于</font><font style="color:rgb(60, 60, 67);"> </font>`padding`<br/><font style="color:rgb(60, 60, 67);"> </font><font style="color:rgb(60, 60, 67);">与</font><font style="color:rgb(60, 60, 67);"> </font>`margin` | <font style="color:rgb(60, 60, 67);">string</font> | `32` |
| <font style="color:rgb(60, 60, 67);">fontSizeXs</font> | <font style="color:rgb(60, 60, 67);">文字大小</font> | <font style="color:rgb(60, 60, 67);">string</font> | `10` |
| <font style="color:rgb(60, 60, 67);">fontSizeSm</font> | <font style="color:rgb(60, 60, 67);">文字大小</font> | <font style="color:rgb(60, 60, 67);">string</font> | `12` |
| <font style="color:rgb(60, 60, 67);">fontSizeMd</font> | <font style="color:rgb(60, 60, 67);">文字大小</font> | <font style="color:rgb(60, 60, 67);">string</font> | `14` |
| <font style="color:rgb(60, 60, 67);">fontSizeLg</font> | <font style="color:rgb(60, 60, 67);">文字大小</font> | <font style="color:rgb(60, 60, 67);">string</font> | `16` |
| <font style="color:rgb(60, 60, 67);">fontSizeXl</font> | <font style="color:rgb(60, 60, 67);">文字大小</font> | <font style="color:rgb(60, 60, 67);">string</font> | `20` |
| <font style="color:rgb(60, 60, 67);">borderRadiusSm</font> | <font style="color:rgb(60, 60, 67);">圆角大小</font> | <font style="color:rgb(60, 60, 67);">string</font> | `2` |
| <font style="color:rgb(60, 60, 67);">borderRadiusMd</font> | <font style="color:rgb(60, 60, 67);">圆角大小</font> | <font style="color:rgb(60, 60, 67);">string</font> | `4` |
| <font style="color:rgb(60, 60, 67);">borderRadiusLg</font> | <font style="color:rgb(60, 60, 67);">圆角大小</font> | <font style="color:rgb(60, 60, 67);">string</font> | `8` |
| <font style="color:rgb(60, 60, 67);">borderRadiusMax</font> | <font style="color:rgb(60, 60, 67);">圆角大小</font> | <font style="color:rgb(60, 60, 67);">string</font> | `9999` |
| <font style="color:rgb(60, 60, 67);">animationDuration</font> | <font style="color:rgb(60, 60, 67);">动画时长，单位</font><font style="color:rgb(60, 60, 67);"> </font>`ms`<br/><font style="color:rgb(60, 60, 67);">，如</font>`Switch`<br/><font style="color:rgb(60, 60, 67);">组件的切换动画时长</font> | <font style="color:rgb(60, 60, 67);">number</font> | <font style="color:rgb(60, 60, 67);">200</font> |


#### 项目启动页面主题设置
+ 在项目的启动页面中，通过调用 `IBestSetUIBaseStyle` 方法来配置主题。
+ 若项目已使用了 `IBestSetGlobalConfig` 方法，请确保在调用该方法之后再调用 `IBestSetUIBaseStyle`。

```typescript
import { IBestSetUIBaseStyle } from "@ibestservices/ibest-ui";
onWindowStageCreate(windowStage: window.WindowStage): void {
    windowStage.loadContent('pages/Index', (err, data) => {
        IBestSetUIBaseStyle({
            primary: '#7232dd'
        });
    });
}
```

#### `IBestUIBaseStyleObjType` 类型说明
+ 此类型定义了 `IBestSetUIBaseStyle` 方法的参数结构。所有属性均为可选，传入值将覆盖默认设置。随着组件库的不断扩展，支持的预设样式也将逐渐增加。
+ 默认单位为 `vp`；若全局单位设置为 `lpx`，则以下数值将根据 `designWidth` 和当前设备屏幕宽度自动转换为相应的 `lpx` 视窗尺寸。

## 四、结语
通过本文的实践，开发者可以迅速掌握基于HarmonyOS Next的操作系统上构建IBest-UI用户界面的方法与技巧。这不仅涵盖了基础的搭建流程，还包括了一些实用的设计模式和最佳实践，旨在帮助初学者快速入门，并为有一定经验的开发者提供参考。为了更全面地理解和应用这一开发体系，我们建议读者结合官方文档进行学习，特别是那些涉及高级特性的部分，如性能优化、复杂布局管理等，这些内容对于创建高质量的应用程序至关重要。  
	未来，我将继续推出一系列进阶教程，包括但不限于组件自定义方法、如何实现跨设备兼容性（即多端适配）等方面的深入探讨。此外，还将介绍一些最新的技术和工具，帮助开发者紧跟技术趋势，不断提升自己的技能水平。无论是希望深化现有知识还是探索新领域的开发者，都能从中受益匪浅。因此，请大家保持关注，不要错过任何更新！

