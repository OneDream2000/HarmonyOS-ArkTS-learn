## 1. Introduction: Let's Dive into the Source Code and Unlock the "Component Secrets" of HarmonyOS NEXT!
Hey everyone! Today, I want to talk about a super practical open-source project — **IBest-UI**, a lightweight UI component library specially designed for the HarmonyOS ecosystem.

If you're developing applications on HarmonyOS NEXT, you've probably encountered these pain points:

+ Repeating boilerplate code
+ Time-consuming cross-device layout adaptation
+ Complex handling of dark/light mode switching

Well, worry no more — **IBest-UI** is here to save the day!

### Why is it so great?
+ **Lightweight**: Minimal core code, ready to use without bloating your app.
+ **Theme Flexibility**: Switch between light and dark modes in one line of code, fully compatible with HarmonyOS Meta Services.
+ **Beautiful UI Components**: From buttons to popups, from badges to navigation bars, covering high-frequency scenarios. The design style draws inspiration from Vant — known for its elegant and clean interface.

But today, we’re not just going to use the components — we’re going to **open the hood** and explore the “black magic” inside!

We’re launching a **source code reading plan** with a simple goal:

+ **Decompose Design Philosophy**: How does the Badge component achieve dynamic updates and responsive layouts?
+ **Learn from IBest-UI**: Discover advanced ArkTS patterns used in HarmonyOS NEXT for declarative UI development.
+ **Learn by Doing**: Feel free to ask questions or submit PRs — let’s make IBest-UI even better together!

Whether you're looking to improve your source code reading skills or master the art of HarmonyOS development, this series will be your **practical guide**. Ready to dig in? Let’s go! 🚀

---

## 2. Prerequisites
When exploring an open-source project, the first step is always to read the [README.md](https://github.com/ibestservices/ibest-ui/blob/master/README.md) and contribution guide [CONTRIBUTING.md](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fyouzan%2Fvant%2Fblob%2Fmain%2F.github%2FCONTRIBUTING.md).

### Clone the Repository
```bash
# Clone from GitHub
git clone git@github.com:ibestservices/ibest-ui.git

# Or clone from Gitee
git clone git@gitee.com:ibestservices/ibest-ui.git

# Enter the project directory
cd ./ibest-ui

# Install dependencies
ohpm install
```

### Directory Structure
Based on the contribution documentation, the main structure looks like this:

```plain
├── entry # Example hap package
│   └── src
│       ├── main
│           ├── ets
│               ├── assets
│               ├── components
│               ├── entryability
│               └── pages # Example pages
│           └── resources
├── hvigor
├── library # Component library
│   └── src
│       └── main
│           ├── ets
│               ├── assets
│               ├── components # Component directory
│               │   ├── button
│               │   ├── cell
│               │   └── ...
│               └── theme-chalk # Global styles
│           └── resources
```

From this structure, we can see that most development happens in the `library` folder, while `entry/main/ets/pages` contains example implementations of each component.

Global style variables are defined in `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library/src/theme-chalk/...</font>`.

---

## 3. Quickly Locate the Source Code
Use the shortcut `Ctrl+Shift+N` to navigate to files quickly.

According to the earlier directory structure, we know that:

+ `entry` contains example files
+ `library` is where the actual component library resides

So, the badge component should be located at:

```plain
<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\ets\components\badge</font>

```

---

## 4. Source Code Analysis
Inside the badge directory, there are three key files:

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742398817117-db21cf96-f4c0-47f4-877a-787a9a341c67.png)

### `color.ets` File
This file defines related styles, such as retrieving global color settings from:

```typescript
import { IBestBadgeColorType } from "./index.type"

export const IBestBadgeColor: IBestBadgeColorType = {
    badgeBgColor: $r("app.color.ibest_badge_background"),
    textColor: $r("app.color.ibest_badge_text_color")
}
```

### `index.type.ets` File
Defines the `IBestBadgePosition` type:

```typescript
/**
 * Badge position
 */
export type IBestBadgePosition = "top-left" | "top-right" | "bottom-left" | "bottom-right"
```

### Main File: `index.ets`
Here's the full implementation of the `IBestBadge` component:

```typescript
// Import global styles
import { getDefaultBaseStyle, IBEST_UI_NAMESPACE } from "../../theme-chalk/src"
import { IBestUIBaseStyleObjType } from "../../theme-chalk/src/index.type"
// Utility for unit conversion
import { convertDimensionsWidthUnit } from "../../utils/utils"
// Import colors and types
import { IBestBadgeColor } from "./color"
import { IBestBadgePosition } from "./index.type"

@Component
export struct IBestBadge {
    @StorageLink(IBEST_UI_NAMESPACE) baseStyle: IBestUIBaseStyleObjType = getDefaultBaseStyle()
    @Prop content: string | number = ''
    @Prop color: ResourceColor = IBestBadgeColor.badgeBgColor
    @Prop dot: boolean = false
    @Prop max: number = -1
    @Prop showZero: boolean = true
    @Prop badgePosition: IBestBadgePosition = 'top-right'
    @BuilderParam defaultBuilder?: CustomBuilder

    // Determine if badge should be shown
    get isShow(): boolean {
        return !(typeof this.content === "number" && this.content === 0 && !this.showZero)
    }

    // Get displayed content based on max value
    get getContent(): string {
        if (typeof this.content === 'number' && this.max > 0 && this.content > this.max) {
            return `${this.max}+`
        }
        return this.content.toString()
    }

    // Get position based on badgePosition
    getPosition(): Edges {
        switch (this.badgePosition) {
            case 'top-left':
                return { left: 0, top: 0 }
            case 'top-right':
                return { right: 0, top: 0 }
            case 'bottom-left':
                return { left: 0, bottom: 0 }
            case 'bottom-right':
                return { right: 0, bottom: 0 }
        }
    }

    // Get translation values for positioning
    getTranslate(): TranslateOptions {
        switch (this.badgePosition) {
            case 'top-left':
                return { x: "-50%", y: "-50%" }
            case 'top-right':
                return { x: "50%", y: "-50%" }
            case 'bottom-left':
                return { x: "-50%", y: "50%" }
            case 'bottom-right':
                return { x: "50%", y: "50%" }
        }
    }

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
            } else if (this.isShow) {
                Text(this.getContent)
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

---

## 5. Code Explanation
This code implements a `IBestBadge` component that displays a badge with customizable content, background color, and positioning.

### Property Definitions
| Property | Description | Type | Default Value |
| --- | --- | --- | --- |
| `baseStyle` | Global style via `@StorageLink` | `IBestUIBaseStyleObjType` | Default Theme |
| `content` | Badge content | `string | number` |
| `color` | Background color | `ResourceColor` | `badgeBgColor` |
| `dot` | Display as small red dot | `boolean` | `false` |
| `max` | Max value before showing `X+` | `number` | `-1` |
| `showZero` | Show badge when content is zero | `boolean` | `true` |
| `badgePosition` | Positioning (top-left/right etc.) | `IBestBadgePosition` | `top-right` |
| `defaultBuilder` | Custom content builder | `CustomBuilder` | `undefined` |


### Method Logic
+ `isShow()` – Determines whether the badge should be visible.
+ `getContent()` – Returns badge content, appending `+` if over max.
+ `getPosition()` – Calculates position based on direction.
+ `getTranslate()` – Calculates translation offsets.
+ `build()` – Renders either custom content, a dot, or standard badge text.

---

## 6. Customizing Themes
The `baseStyle` property uses `@StorageLink`, which enables two-way data binding with `AppStorage`. This allows global style changes to propagate across all components.

### Understanding `@StorageLink`
> `@StorageLink(key)` establishes a two-way sync between the decorated variable and the AppStorage key:
>
> + Local changes update AppStorage.
> + AppStorage changes update all bound properties.
>

### Initializing `AppStorage`
```typescript
export const IBEST_UI_NAMESPACE = '__IBEST-UI_BASE_STYLE'

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

This method merges provided styles with defaults and stores them globally using `AppStorage`.

---

## 7. Conclusion
In this article, we took a deep dive into the **IBestBadge** component in the **IBest-UI** component library, uncovering how it leverages HarmonyOS NEXT features to create reusable, customizable UI elements.

### What Did We Learn?
+ **Component Usage**: Displays numbers/text, supports dots, truncates overflow (like `99+`), and positions itself in four corners.
+ **Key Decorators**: Used `@StorageLink` for global theme synchronization — change once, apply everywhere!
+ **Core Properties**: `content`, `dot`, `max`, and `badgePosition` allow flexible customization.
+ **Dynamic Positioning**: `getPosition()` and `getTranslate()` handle offset calculations automatically.
+ **Conditional Rendering**: `isShow()` controls visibility logic — including zero-value toggling.
+ **Theme Management**: Centralized styling via `setIBestUIBaseStyle()` ensures consistent look and feel.

### Hidden Gems
+ **ArkTS Magic**: The `convertDimensionsWidthUnit` utility handles device-specific unit conversions — a key part of responsive design in HarmonyOS.
+ **More to Explore**: The component library includes many more components like buttons and navigation bars — Badge was just the appetizer!

### Final Thoughts
This component library demonstrates a solid understanding of HarmonyOS’s declarative UI paradigm. By following its patterns, developers can reduce boilerplate and improve maintainability. Highly recommended for learning and production use!

