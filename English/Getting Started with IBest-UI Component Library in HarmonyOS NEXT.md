## 1. Introduction
IBest-UI is a lightweight, easy-to-use, customizable UI component library for HarmonyOS that supports both dark and light modes. It is fully compatible with Meta Services and offers flexibility for developers to customize themes and styles according to their needs.

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1741966864471-aef6aba6-0b52-4166-a642-47e73da02ce3.png)

---

## 2. Setting Up the Development Environment for HarmonyOS NEXT
### Configure Development Tools
1. **Download and Install DevEco Studio**  
[DevEco Studio Download Link](https://developer.huawei.com/consumer/cn/deveco-studio/)
2. **Configure SDK**  
    - Go to `Tools > SDK Manager`
    - Check and install **HarmonyOS Next 5.0.0 (API 12)** or later versions
3. **Verify Environment Setup**  
    - Create a new project using the **Empty Ability** template
    - Select the **HarmonyOS** template during setup

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1741967476155-89b0d10a-b36b-4d01-8680-89ed303f0e91.png)

---

## 3. Installation and Usage
### Step 1: Install Dependency
```bash
ohpm install @ibestservices/ibest-ui
```

### Step 2: Initialize the Component Library
In your project’s entry file (`entryability/EntryAbility.ets`), initialize the component library:

```typescript
import { IBestInit } from "@ibestservices/ibest-ui"

onWindowStageCreate(windowStage: window.WindowStage): void {
  windowStage.loadContent('pages/Index', (err, data) => {
    // Initialize the component library here!!!
    IBestInit(windowStage, this.context)
  })
}
```

### Step 3: Use Components
Import and use the components you need in your pages:

```typescript
import { IBestButton } from "@ibestservices/ibest-ui"

@Entry
@Component
struct Index {
  @State message: string = 'Hello World'

  build() {
    Column() {
      IBestButton({
        text: "Primary Button",
        type: 'primary'
      })
      IBestButton({
        text: "Success Button",
        type: 'success'
      })
      IBestButton({
        text: "Default Button",
        type: 'default'
      })
      IBestButton({
        text: "Danger Button",
        type: 'danger'
      })
      IBestButton({
        text: "Warning Button",
        type: 'warning'
      })
    }
    .height('100%')
    .width('100%')
  }
}
```

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1741968559657-ee6915f0-3cf4-40b3-9a88-55cea9336fe0.png)

---

## 4. Advanced Features
### 1. Global Configuration
The component library provides global configuration options for developers to choose appropriate dimension units based on their project requirements.

#### How to Use
:::info
Make sure to call this **after initializing the component library**.

:::

```typescript
import { IBestSetGlobalConfig } from "@ibestservices/ibest-ui"

onWindowStageCreate(windowStage: window.WindowStage): void {
  windowStage.loadContent('pages/Index', (err, data) => {
    IBestInit(windowStage, this.context)
    IBestSetGlobalConfig({
      unit: "lpx",
      fontUnit: "lpx"
    })
  })
}
```

#### `IBestGlobalConfigType` Parameters
| Parameter | Description | Type | Default |
| --- | --- | --- | --- |
| designWidth | Base width of the design layout | number | 720 |
| unit | Global size unit (vp, px, lpx) | string | vp |
| fontUnit | Font size unit (vp, px, lpx, fp) | string | fp |


---

### 2. Customizing Theme Styles
To change the theme style, use the `IBestSetUIBaseStyle` method.

#### Changing the Theme
+ Call `IBestSetUIBaseStyle` in your app's launch page.
+ If you're also using `IBestSetGlobalConfig`, make sure to call it **after** that method.

```typescript
import { IBestSetUIBaseStyle } from "@ibestservices/ibest-ui"

onWindowStageCreate(windowStage: window.WindowStage): void {
  windowStage.loadContent('pages/Index', (err, data) => {
    IBestSetUIBaseStyle({
      primary: '#7232dd'
    })
  })
}
```

#### `IBestUIBaseStyleObjType` Parameters
All properties are optional and will override default values if provided.

| Parameter | Description | Type | Default |
| --- | --- | --- | --- |
| primary | Primary feedback color | string | #3D8AF2 |
| success | Success feedback color | string | #58DB6B |
| warning | Warning feedback color | string | #F29C73 |
| danger | Danger feedback color | string | #DB3131 |
| default | Default background color | string | #FFFFFF |
| spaceMini | Mini spacing (padding/margin) | string | 2 |
| spaceBase | Basic spacing | string | 4 |
| spaceXs | Extra small spacing | string | 8 |
| spaceSm | Small spacing | string | 12 |
| spaceMd | Medium spacing | string | 16 |
| spaceLg | Large spacing | string | 24 |
| spaceXl | Extra large spacing | string | 32 |
| fontSizeXs | Extra small font size | string | 10 |
| fontSizeSm | Small font size | string | 12 |
| fontSizeMd | Medium font size | string | 14 |
| fontSizeLg | Large font size | string | 16 |
| fontSizeXl | Extra large font size | string | 20 |
| borderRadiusSm | Small border radius | string | 2 |
| borderRadiusMd | Medium border radius | string | 4 |
| borderRadiusLg | Large border radius | string | 8 |
| borderRadiusMax | Maximum border radius | string | 9999 |
| animationDuration | Animation duration in milliseconds | number | 200 |


> Note: The default unit is `vp`. If you set the global unit to `lpx`, values will be automatically adjusted based on the screen size and design width.
>

---

## 5. Conclusion
Through this guide, developers can quickly learn how to build user interfaces using the **IBest-UI** component library on HarmonyOS NEXT. This includes not only basic setup but also practical design patterns and best practices to help both beginners and experienced developers streamline their development workflow.

We recommend referring to the official documentation for more advanced topics such as performance optimization and complex layout management, which are crucial for building high-quality applications.

In future tutorials, we will dive deeper into advanced topics like component customization, cross-device compatibility (multi-end adaptation), and the latest tools and technologies to help developers stay ahead of the curve.

Stay tuned for more updates!

