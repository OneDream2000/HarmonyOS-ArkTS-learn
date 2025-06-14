## Introduction
In HarmonyOS NEXT, how can we achieve functionality similar to Vue-Slot or React-RenderProps? That is, allowing the passing of UI structure functions (functions decorated with `@Builder`) as parameters to components and rendering them at specific locations within the component. This can be done using the `@BuilderParam` decorator.

### The `@BuilderParam` Decorator
The `@BuilderParam` decorator is used to mark variables pointing to `@Builder` methods. When initializing a custom component, assigning a value to this property allows us to add specific functionalities to the component. This decorator is used to declare an element for any UI description, and its function is similar to the `slot` placeholder in Vue.

---

## Implementing Slots
### Steps:
1. **Declare **`@BuilderParam`** Property in Subcomponent**  
2. **Pass **`@Builder`** Function or Arrow Function from Parent Component**  
3. **Invoke the Logic of the **`@Builder`** Function**

:::info
You may choose not to provide an initial value for the `BuilderParam`. If you do, it will serve as the default content when no content is provided.

:::

---

### Custom Component
```typescript
// Subcomponent
@Component
export struct Card {
  // Declaring the slot to be passed!
  @Builder
  slot() {
    Text('No data available').margin(30)
  }

  private title: ResourceStr
  @BuilderParam component: () => void = this.slot

  build() {
    Column() {
      Row() {
        Text(this.title)
          .fontColor('#333333')
          .fontSize(18)
          .fontWeight(700)
          .lineHeight(26)
      }.justifyContent(FlexAlign.Start).width('100%')

      this.component()
    }
    .backgroundColor(Color.White)
    .width('100%')
    .padding({ left: 16, right: 16, top: 20, bottom: 20 })
    .borderRadius(12)
    .margin({ bottom: 12 })
  }
}
```

---

### External Invocation
Below are several examples demonstrating usage:

```typescript
@Builder
function overBuilder() {
  Text('External Component').margin(30)
}

// Invoking from parent layout:
@Preview
@Component
@Entry
export struct Index {
  @Builder
  componentBuilder() {
    Text('Internal Component').margin(30)
  }

  build() {
    Column() {
      Card({ title: 'Default Slot' })
      Card({ title: 'Using Internal Component Slot', component: this.componentBuilder })
      Card({ title: 'Using External Component Slot', component: overBuilder })
      Card({ title: 'Direct Value Passing' }) {
        Text('Direct Value').margin(30)
      }
    }.height('100%')
     .padding({ left: 16, right: 16, top: 20, bottom: 20 })
     .backgroundColor('#f0f3f6')
  }
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/26122809/1718526482626-5303323b-a565-4d3e-95b9-0fd8dfde0b1d.png)

:::info
When there's only one `BuilderParam` and no parameters need to be passed, you can use trailing closures.  
Note: An empty trailing closure `{}` means passing an empty content, which will replace the default content.

:::

The above examples all demonstrate single slots. What if we want to use multiple slots?

---

## Multiple Slots Usage
When the subcomponent has multiple `BuilderParam`s, they must be passed through parameters.

### Custom Component
```typescript
// Subcomponent
@Component
export struct Card {
  @Builder
  slot() {
    Text('No data available').margin(30)
  }

  @Builder
  defaultRightSlot() {
    Text('Details')
  }

  private title: ResourceStr
  @BuilderParam component: () => void = this.slot
  @BuilderParam rightSlot: () => void = this.defaultRightSlot

  build() {
    Column() {
      Row() {
        Text(this.title)
          .fontColor('#333333')
          .fontSize(18)
          .fontWeight(700)
          .lineHeight(26)
        this.rightSlot()
      }.justifyContent(FlexAlign.SpaceBetween).width('100%')

      this.component()
    }
    .backgroundColor(Color.White)
    .width('100%')
    .padding({ left: 16, right: 16, top: 20, bottom: 20 })
    .borderRadius(12)
    .margin({ bottom: 12 })
  }
}
```

---

### External Invocation
```typescript
@Builder
function overBuilder() {
  Text('External Component').margin(30)
}

// Invoking from parent layout:
@Preview
@Component
@Entry
export struct Index {
  @Builder
  componentBuilder() {
    Text('Internal Component').margin(30)
  }

  @Builder
  rightBuilder() {
    Text('Right Slot Content').margin(30)
  }

  build() {
    Column() {
      Card({ title: 'Default Slot' })
      Card({ title: 'Passing Through Parameters', component: this.componentBuilder, rightSlot: this.rightBuilder })
      // Card({ title: 'This will cause an error' }) {
      //   Text('Direct Value').margin(30)
      // }
    }.height('100%')
     .padding({ left: 16, right: 16, top: 20, bottom: 20 })
     .backgroundColor('#f0f3f6')
  }
}
```

![](https://cdn.nlark.com/yuque/0/2024/png/26122809/1719161176512-12969bbe-213a-42de-bf42-8ae93b961acb.png)

There are many ways to implement slots. For more details, please refer to the official documentation: [**@BuilderParam Decorator: Referencing @Builder Functions**](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/arkts-builderparam-0000001524416541-V2)

---

## Mandatory Parameter Passing
If you want the parent component to **must pass** a property when using your encapsulated component, you can use the `Require` modifier.

:::info
The `Require` modifier can only be applied before two modifiers: `Prop` and `BuilderParam`.

:::

### Custom Component
```typescript
@Component
export struct Card {
  @Builder
  slot() {
    Text('No data available').margin(30)
  }

  @Builder
  defaultRightSlot() {
    Text('Details')
  }

  private title: ResourceStr
  // The parent component must pass this; otherwise, an error occurs
  @Require
  @BuilderParam component: () => void = this.slot
  @BuilderParam rightSlot: () => void = this.defaultRightSlot

  build() {
    Column() {
      Row() {
        Text(this.title)
          .fontColor('#333333')
          .fontSize(18)
          .fontWeight(700)
          .lineHeight(26)
        this.rightSlot()
      }.justifyContent(FlexAlign.SpaceBetween).width('100%')

      this.component()
    }
    .backgroundColor(Color.White)
    .width('100%')
    .padding({ left: 16, right: 16, top: 20, bottom: 20 })
    .borderRadius(12)
    .margin({ bottom: 12 })
  }
}
```

---

### External Invocation
```typescript
@Builder
function overBuilder() {
  Text('External Component').margin(30)
}

// Invoking from parent layout:
@Preview
@Component
@Entry
export struct Index {
  @Builder
  componentBuilder() {
    Text('Internal Component').margin(30)
  }

  @Builder
  rightBuilder() {
    Text('Right Slot Content').margin(30)
  }

  build() {
    Column() {
      // No component passed, which will result in an error
      Card({ title: 'Passing Through Parameters', rightSlot: this.rightBuilder })
    }.height('100%')
     .padding({ left: 16, right: 16, top: 20, bottom: 20 })
     .backgroundColor('#f0f3f6')
  }
}
```

For more information on implementing slots, please refer to the official documentation: [**@BuilderParam Decorator: Referencing @Builder Functions**](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/arkts-builderparam-0000001524416541-V2)

