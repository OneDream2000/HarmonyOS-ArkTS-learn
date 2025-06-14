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

+ **Decompose Design Philosophy**: How does the `TextEllipsis` component achieve dynamic text truncation and responsive layouts?
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

## 3. Demo Preview
You can quickly navigate to files using the shortcut `Ctrl+Shift+N`.

According to the earlier directory structure, we know that:

+ `entry` contains example files
+ `library` is where the actual component library resides

So, the badge component should be located at:

```plain
<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">entry\src\main\ets\pages\base\TextEllipsis.ets</font>

```

Here's a demo implementation:

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
    @State text: string = 'Most of life is平淡无奇. This is necessary for physical and mental recovery. Only through calmness can we prepare for the next peak.'

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
                    ComponentShowContainer({ title: 'Basic Usage' }) {
                        IBestTextEllipsis({
                            text: this.text
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: 'Expand/Collapse' }) {
                        IBestTextEllipsis({
                            text: $r("app.string.app_desc"),
                            showAction: true
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: 'Custom Line Count' }) {
                        Column({ space: SPACE.SM }) {
                            IBestTextEllipsis({
                                text: this.text,
                                showAction: true,
                                rows: 3
                            })
                            Row() {
                                IBestTextEllipsis({
                                    text: this.text,
                                    showAction: true,
                                    rows: 3
                                })
                            }.width(200)
                        }
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: 'Custom Ellipsis Position' }) {
                        ComponentShowContainer({ title: 'Start Truncation' }) {
                            IBestTextEllipsis({
                                text: this.text,
                                showAction: true,
                                omitPosition: 'start'
                            })
                        }
                        ComponentShowContainer({ title: 'Middle Truncation' }) {
                            IBestTextEllipsis({
                                text: this.text,
                                omitPosition: 'middle',
                                showAction: true,
                                rows: 2
                            })
                        }
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: 'Custom Ellipsis Content' }) {
                        IBestTextEllipsis({
                            text: this.text,
                            omitContent: "•••"
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: 'Custom Action Style' }) {
                        IBestTextEllipsis({
                            text: this.text,
                            showAction: true,
                            expandText: "Expand",
                            collapseText: "Collapse",
                            actionColor: "#DB3131"
                        })
                    }
                }
            }
            .layoutWeight(1)
            .padding({ left: SPACE.SM, right: SPACE.SM })
        }
        .width(CONTAINER_SIZE.FULL)
        .height(CONTAINER_SIZE.FULL)
        .backgroundColor(modeColor.bg2)
    }
}
```

![](https://cdn.nlark.com/yuque/0/2025/gif/26122809/1742713948933-8a6a5590-d1c7-42b8-b8c7-1be5943cc32a.gif)

---

## 4. Source Code Analysis
To locate the source code, navigate to:

```plain
<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\ets\components\textEllipsis</font>

```

Inside the directory, there are two key files:

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742714075224-707c8912-bd25-47aa-b34b-961f7d37baf6.png)

### `color.ets` File
This file defines related styles:

```typescript
interface IBestTextEllipsisColorType {
    textColor: ResourceColor
}

export const IBestTextEllipsisColor: IBestTextEllipsisColorType = {
    textColor: $r("app.color.ibest_text_color")
}
```

### Main File: `index.ets`
Here is the full implementation of the `IBestTextEllipsis` component:

```typescript
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
 * A text ellipsis component supporting multi-line truncation and action buttons
 */
@Component
export struct IBestTextEllipsis {
  /**
   * Global public style loaded from storage
   */
  @StorageLink(IBEST_UI_NAMESPACE) baseStyle: IBestUIBaseStyleObjType = getDefaultBaseStyle()

  /**
   * Displayed text content
   */
  @Prop @Watch("formatText") text: ResourceStr = ""

  /**
   * Font size, default is medium font size from global style
   */
  @Prop textFontSize: IBestStringNumber = this.baseStyle.fontSizeMd as string

  /**
   * Text color, default is component-defined color
   */
  @Prop textColor: ResourceColor = IBestTextEllipsisColor.textColor

  /**
   * Line height, default is 20px
   */
  @Prop lineHeight: IBestStringNumber = convertDimensionsWidthUnit(20)

  /**
   * Number of lines to display, default is 1
   */
  @Prop @Watch("formatText") rows: number = 1

  /**
   * Whether to show expand/collapse buttons
   */
  @Prop showAction: boolean = false

  /**
   * Expand action text, default is resource string
   */
  @Prop expandText: ResourceStr = $r("app.string.ibest_text_expand")

  /**
   * Collapse action text, default is resource string
   */
  @Prop collapseText: ResourceStr = $r("app.string.ibest_text_collapse")

  /**
   * Ellipsis content, default is "…"
   */
  @Prop omitContent: ResourceStr = "…"

  /**
   * Action text color, default is primary color from global style
   */
  @Prop actionColor: ResourceColor = this.baseStyle.primary

  /**
   * Ellipsis position, options: "start", "middle", "end", default is "end"
   */
  @Prop @Watch("formatText") omitPosition: "start" | "middle" | "end" = "end"

  /**
   * Component state variables
   */
  @State uniId: number = 0 // Unique identifier
  @State showText: string = "" // Current displayed text
  @State isExpand: boolean = false // Whether expanded
  @State textWidth: number = 0 // Text width
  @State textHeight: number = 0 // Text height
  @State maxLineHeight: number = 0 // Max line height

  /**
   * Private property: Get UI context
   */
  private uiContext = this.getUIContext()

  /**
   * Builder for ellipsis content
   */
  @Builder
  OmitContent() {
    Span(this.omitContent)
      .fontColor(this.textColor)
      .fontSize(getSizeByUnit(this.textFontSize, true))
  }

  /**
   * Triggered before component appears, initializes unique ID and formats text
   */
  aboutToAppear(): void {
    this.uniId = this.getUniqueId()
    this.formatText()
  }

  /**
   * Get text string from resources
   * @returns Text string
   */
  getTextString() {
    return getResourceStr(this.text)
  }

  /**
   * Get ellipsis text from resources
   * @returns Ellipsis text
   */
  getOmitText() {
    return getResourceStr(this.omitContent)
  }

  /**
   * Format text based on container width and row count
   */
  formatText() {
    setTimeout(() => {
      this.textWidth = getComponentsInfo(this.uiContext, `ibest_text_${this.uniId}`).width
      this.textHeight = this.measureText(this.getTextString())
      this.maxLineHeight = this.measureText(this.getTextString(), this.rows)

      if (this.textHeight > this.maxLineHeight) {
        this.getTextByWidth()
      } else {
        this.showText = this.getTextString()
      }
    }, 0)
  }

  /**
   * Truncate text based on width, supports different ellipsis positions
   */
  getTextByWidth() {
    let clipText = this.getTextString()
    let textHeight = this.textHeight
    let centerIndex = Math.floor(clipText.length / 2)
    let leftStr = clipText.slice(0, centerIndex)
    let rightStr = clipText.slice(centerIndex)
    let omitText = this.getOmitText()
    let expandText = this.getExpandText()

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

    this.showText = this.omitPosition == 'middle' ? leftStr + omitText + rightStr : clipText
  }

  /**
   * Measure text height
   * @param text Text content
   * @param rows Maximum number of lines
   * @returns Text height
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
   * Build component UI
   */
  build() {
    Text() {
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

---

## 5. Code Explanation
### 1. Importing Essential Tools and Types
The component imports essential tools and types such as:

+ `IBestStringNumber`: For defining numeric values like font sizes.
+ `getDefaultBaseStyle` and `IBEST_UI_NAMESPACE`: For loading global styles.
+ Utility functions like `convertDimensionsWidthUnit`, `getComponentsInfo`, etc., for unit conversion and component info retrieval.

These tools provide foundational support for implementing responsive design.

### 2. Defining Component Structure
#### Props (Properties)
| Property | Description | Type | Default Value |
| --- | --- | --- | --- |
| `text` | Displayed text content | `ResourceStr` | Empty String |
| `textFontSize` | Font size | `IBestStringNumber` | Medium Size |
| `textColor` | Text color | `ResourceColor` | Defined Color |
| `lineHeight` | Line height | `IBestStringNumber` | 20px |
| `rows` | Number of lines | `number` | 1 |
| `showAction` | Show expand/collapse button | `boolean` | `false` |
| `expandText` | Expand button text | `ResourceStr` | "Expand" |
| `collapseText` | Collapse button text | `ResourceStr` | "Collapse" |
| `omitContent` | Ellipsis content | `ResourceStr` | "…" |
| `actionColor` | Action button color | `ResourceColor` | Primary Color |
| `omitPosition` | Ellipsis position (start/middle/end) | `"start" | "middle" |


#### State (Internal Variables)
| Variable | Description |
| --- | --- |
| `uniId` | Unique identifier |
| `showText` | Currently displayed text |
| `isExpand` | Whether expanded |
| `textWidth` | Width of the text area |
| `textHeight` | Height of the full text |
| `maxLineHeight` | Maximum allowed height |


#### Private Properties
+ `uiContext`: UI context for measuring text dimensions.

#### Builders
+ `OmitContent`: Builds the ellipsis symbol with correct styling.

---

## 6. Core Methods
### `aboutToAppear`
Initializes unique ID and triggers text formatting.

### `getResourceStr`
Converts resource strings to actual values.

### `getTextString`, `getOmitText`, `getExpandText`
Retrieve formatted text content for display.

### `formatText`
Measures text dimensions and determines whether to truncate.

### `getTextByWidth`
Truncates text based on position (`start`, `middle`, `end`) until it fits.

### `measureText`
Measures text height for accurate truncation logic.

---

## 7. Building the UI
The `build()` method dynamically constructs the UI based on current settings:

+ Displays ellipsis at start or end if needed.
+ Shows either truncated or full text.
+ Renders expand/collapse button when enabled.

---

## 8. Control Flow Diagram
![](https://cdn.nlark.com/yuque/__mermaid_v3/9bc4b47245c3f4668565713b602beb53.svg)

---

## 9. Conclusion
In this article, we explored the **IBestTextEllipsis** component from the **IBest-UI** component library. This component provides:

+ Dynamic text truncation based on container width and line count.
+ Support for ellipsis positioning (start, middle, end).
+ Expandable/collapsible content toggle.
+ Customizable font size, color, and line height.

With this component, developers can easily implement multi-line text truncation while ensuring a smooth user experience.

Whether you're dealing with long descriptions, product details, or any scenario involving lengthy text, this component offers a robust solution.

