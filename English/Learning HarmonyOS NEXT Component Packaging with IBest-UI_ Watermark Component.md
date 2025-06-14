## 1. Introduction
In this article, we will explore the **Watermark** component from the **IBest-UI** component library — a lightweight and customizable UI library designed for HarmonyOS.

If you're developing applications on HarmonyOS NEXT, you may have encountered the need to:

+ Overlay watermarks on pages or components
+ Support both image and text-based watermarks
+ Customize watermark styles (font size, color, rotation, spacing, etc.)

The `IBestWatermark` component is here to help!

### Why is it so great?
+ **Flexible Watermark Types**: Supports both image and text watermarks.
+ **Customizable Styles**: Allows customization of font size, font family, color, rotation angle, and spacing.
+ **Layer Control**: Provides options to set z-index for proper layering.
+ **Reusability**: Can be applied across multiple pages or components easily.

Whether you're protecting content, adding branding elements, or enhancing security, this component offers a robust solution.

---

## 2. Prerequisites
Before diving into the component, make sure you’ve read the [README.md](https://github.com/ibestservices/ibest-ui/blob/master/README.md) and contribution guide [CONTRIBUTING.md](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fyouzan%2Fvant%2Fblob%2Fmain%2F.github%2FCONTRIBUTING.md).

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
Based on the documentation:

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

Most development occurs in the `library` folder, while `entry/main/ets/pages` contains example implementations of each component.

Global style variables are defined in `<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library/src/theme-chalk/...</font>`.

---

## 3. Demo Preview
You can quickly navigate to files using the shortcut `Ctrl+Shift+N`.

According to the earlier directory structure, we know that:

+ `entry` contains example files
+ `library` is where the actual component library resides

So, the watermark component should be located at:

```plain
<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\ets\components\watermark</font>

```

Here's an example usage:

```typescript
import router from '@ohos.router';
import { CONTAINER_SIZE, modeColor, SPACE } from '../../assets/styles/BaseStyle';
import { ComponentRouterParams } from '../../assets/global.type';
import ComponentShowContainer from '../../components/ComponentShowContainer';
import { IBestNavBar, IBestWatermark } from '@ibestservices/ibest-ui';

@Entry
@Component
struct WatermarkPage {
    @State title: string = (router.getParams() as ComponentRouterParams).title || ''

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
                        IBestWatermark({
                            text: "IBest-UI",
                            fontSize: 16
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: 'Image Watermark' }) {
                        IBestWatermark({
                            src: $r("app.media.ibest_logo"),
                            width: 60,
                            height: 60
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: 'Custom Rotation' }) {
                        IBestWatermark({
                            text: "Rotated Watermark",
                            rotateDeg: -30,
                            fontSize: 14
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: 'Custom Spacing' }) {
                        IBestWatermark({
                            text: "Spaced Watermark",
                            gapX: 40,
                            gapY: 40
                        })
                    }
                }
                ListItem() {
                    ComponentShowContainer({ title: 'Custom Z-Index' }) {
                        IBestWatermark({
                            text: "Top Layer Watermark",
                            waterMarkZIndex: 9999
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

![](https://cdn.nlark.com/yuque/0/2025/gif/26122809/1742732392255-1af4e3da-7eb1-44e8-b794-5599af03d933.gif)

---

## 4. Source Code Analysis
Navigate to:

```plain
<font style="color:rgb(255, 80, 44);background-color:rgb(255, 245, 245);">library\src\main\ets\components\watermark</font>

```

Inside the directory, there are three key files:

![](https://cdn.nlark.com/yuque/0/2025/png/26122809/1742733781014-fd8e9c8c-9679-4ba4-bb8b-325fb2cbcd40.png)

### `color.ets` File
This file defines related styles:

```typescript
interface IBestWatermarkColorType {
    bgColor: ResourceColor
}

export const IBestWatermarkColor: IBestWatermarkColorType = {
    bgColor: $r("app.color.ibest_water_mark_background")
}
```

### Main File: `index.ets`
Here is the full implementation of the `IBestWatermark` component:

```typescript
import { IBestStringNumber } from '../../model/Global.type'
import { getDefaultBaseStyle, IBEST_UI_NAMESPACE } from '../../theme-chalk/src'
import { IBestUIBaseStyleObjType } from '../../theme-chalk/src/index.type'
import { IBestWatermarkColor } from './color'
import { WatermarkCanvas } from './watermarkCanvas'

/**
 * A watermark component supporting both image and text watermarks
 */
@Component
export struct IBestWatermark {
    /**
     * Global public style loaded from storage
     */
    @StorageLink(IBEST_UI_NAMESPACE) baseStyle: IBestUIBaseStyleObjType = getDefaultBaseStyle()

    /**
     * Image source URL for image watermark
     */
    @Prop src: ResourceStr = ""

    /**
     * Text content for text watermark
     */
    @Prop text: ResourceStr = ""

    /**
     * Font size for text watermark
     */
    @Prop fontSize: number = 14

    /**
     * Font family for text watermark
     */
    @Prop fontFamily: string = "Arial"

    /**
     * Font color for text watermark
     */
    @Prop fontColor: IBestStringNumber | CanvasGradient | CanvasPattern = this.baseStyle.primary

    /**
     * Width of watermark tile
     */
    @Prop width: number = 120

    /**
     * Height of watermark tile
     */
    @Prop height: number = 120

    /**
     * Horizontal spacing between tiles
     */
    @Prop gapX: number = 20

    /**
     * Vertical spacing between tiles
     */
    @Prop gapY: number = 20

    /**
     * Rotation angle in degrees
     */
    @Prop rotateDeg: number = -20

    /**
     * Z-index for watermark layering
     */
    @Prop waterMarkZIndex: number = 1000

    /**
     * Callback when watermark is generated
     */
    @BuilderParam onComplete: (url: string) => void = () => {}

    private canvasRef: WatermarkCanvas = new WatermarkCanvas(this)

    build() {
        this.canvasRef.build()
    }
}
```

---

## 5. Core Implementation: `watermarkCanvas.ets`
This file handles the core logic of drawing the watermark using Canvas:

```typescript
import { IBestWatermark } from './index'

export class WatermarkCanvas {
    private context: CanvasRenderingContext2D
    private props: IBestWatermark

    constructor(props: IBestWatermark) {
        this.props = props
        this.context = props.builderContext.createCanvasContext('watermark')
    }

    build() {
        const { width, height, rotateDeg } = this.props
        const data = {
            width,
            height,
            rotateDeg
        }

        this.drawWatermark(data, () => {
            this.drawTextOrImage(data)
        })
    }

    drawWatermark(data: any, callback: () => void) {
        const countData = this.getXYCount(data.width, data.height)
        const countX = countData.countX
        const countY = countData.countY

        for (let i = 0; i < countY; i++) {
            for (let j = 0; j < countX; j++) {
                const x = width * j + this.props.gapX * j
                const y = height * i + this.props.gapY * i

                this.context.save()
                this.context.translate(x, y)
                this.context.rotate((rotateDeg * Math.PI) / 180)
                callback()
                this.context.restore()
            }
        }

        let url: string = this.context.toDataURL("png")
        if (url !== "data:image/png") {
            this.props.onComplete(url)
        }
    }

    drawTextOrImage(data: any) {
        const { text, src } = this.props
        if (text) {
            this.drawText(data)
        } else if (src) {
            this.drawImage(data)
        }
    }

    drawText(data: any) {
        const { text, fontSize, fontFamily, fontColor } = this.props
        this.context.setFontSize(fontSize)
        this.context.font = fontFamily
        this.context.setFillStyle(fontColor)
        this.context.fillText(text, 0, fontSize)
    }

    drawImage(data: any) {
        const { src } = this.props
        this.context.drawImage(src, 0, 0, data.width, data.height)
    }

    getXYCount(width: number, height: number): { countX: number, countY: number } {
        const screenWidth = windowStage.getWindow().getProperties().windowWidth
        const screenHeight = windowStage.getWindow().getProperties().windowHeight
        const countX = Math.ceil(screenWidth / (width + this.props.gapX))
        const countY = Math.ceil(screenHeight / (height + this.props.gapY))
        return { countX, countY }
    }
}
```

---

## 6. Property Reference
| Property | Description | Type | Default Value |
| --- | --- | --- | --- |
| `src` | Image source URL | `ResourceStr` | Empty String |
| `text` | Text watermark content | `ResourceStr` | Empty String |
| `fontSize` | Font size for text watermark | `number` | 14 |
| `fontFamily` | Font family for text watermark | `string` | Arial |
| `fontColor` | Font color for text watermark | `IBestStringNumber | CanvasGradient |
| `width` | Width of watermark tile | `number` | 120 |
| `height` | Height of watermark tile | `number` | 120 |
| `gapX` | Horizontal spacing between tiles | `number` | 20 |
| `gapY` | Vertical spacing between tiles | `number` | 20 |
| `rotateDeg` | Rotation angle in degrees | `number` | -20 |
| `waterMarkZIndex` | Z-index for watermark layering | `number` | 1000 |


---

## 7. Conclusion
In this article, we explored the **IBestWatermark** component from the **IBest-UI** component library. This component provides:

+ Flexible support for both image and text watermarks.
+ Customization options for font size, color, rotation, and spacing.
+ Efficient rendering using Canvas API.
+ Easy integration via declarative syntax.

With this component, developers can easily add visual watermarks to enhance branding, protect content, or improve UI aesthetics.

Whether you're building internal tools, public-facing apps, or enterprise solutions, this component offers a powerful and reusable solution for watermarking in HarmonyOS NEXT.

