## 引言
在HarmonyOS NEXT中，如何实现类似Vue-Slot或React-RenderProps的功能？即允许将UI结构的函数（被`@Builder`修饰的函数）作为参数传递给组件，并在组件内的指定位置渲染，可以使用`@BuilderParam`装饰器。

## @BuilderParam装饰器
`@BuilderParam`装饰器用于标记指向`@Builder`方法的变量。在初始化自定义组件时，可以通过对这个属性赋值来为组件添加特定的功能。该装饰器用于声明任意UI描述的一个元素，其作用类似于Vue中的`slot`占位符。

## 插槽的实现
### 使用步骤
1. **子组件声明** `@BuilderParam` 属性。
2. **父组件传入** `@Builder`修饰的函数或箭头函数。
3. **调用** `@Builder`函数的逻辑。

:::info
<font style="color:rgb(77, 77, 77);">BuilderParam的参数可以不给初始值，如果给了初始值， 就是没有内容的默认内容</font>

:::

#### <font style="color:rgb(77, 77, 77);">自定义组件</font>
```arkts
// 子组件
@Component
  export struct Card {
    // 声明的一个要传递的内容！
    @Builder
    slot() {
      Text('暂无数据').margin(30)
    };

    private title: ResourceStr
    @BuilderParam component: () => void = this.slot;

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

#### <font style="color:rgb(77, 77, 77);">外部调用</font>
下面案例中，列举了多种样例使用方法

```arkts
@Builder
function overBuilder() {
  Text('外部组件').margin(30)
}

// 父布局调用：
@Preview
@Component
@Entry
export struct Index {
  @Builder
  componentBuilder() {
    Text('内部组件').margin(30)
  }

  build() {
    Column() {
      Card({ title: '默认插槽' })
      Card({ title: '使用内部的组件插槽', component: this.componentBuilder })
      Card({ title: '使用外部的组件插槽', component: overBuilder })
      Card({ title: '直接传值' }) {
        Text('直接传值').margin(30)
      }

    }.height('100%').padding({ left: 16, right: 16, top: 20, bottom: 20 }).backgroundColor('#f0f3f6')
  }
}
```

效果图

![](https://cdn.nlark.com/yuque/0/2024/png/26122809/1718526482626-5303323b-a565-4d3e-95b9-0fd8dfde0b1d.png)

:::info
只有一个BuilderParam且不需要传参的时候，可以使用尾随闭包

注意：尾随闭包用空大括号就代表传递空内容，会替代默认内容

:::

以上的例子，都是单个插槽，那如果想使用多个插槽呢?

## 多个插槽使用
子组件有多个BuilderParam，<font style="color:#DF2A3F;">必须通过参数</font>的方式来传入

**<font style="color:rgb(77, 77, 77);">自定义组件</font>**

```arkts

// 子组件
@Component
export struct Card {
  @Builder
  slot() {
    Text('暂无数据').margin(30)
  };

  @Builder
  defaultRightSlot() {
    Text('详情')
  };

  private title: ResourceStr
  @BuilderParam component: () => void = this.slot;
  @BuilderParam rightSlot: () => void = this.defaultRightSlot;

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

**<font style="color:rgb(77, 77, 77);">外部调用</font>**

```arkts
@Builder
function overBuilder() {
  Text('外部组件').margin(30)
}

// 父布局调用：
@Preview
@Component
@Entry
export struct Index {
  @Builder
  componentBuilder() {
    Text('内部组件').margin(30)
  }

  @Builder
  rightBuilder() {
    Text('右边的插槽').margin(30)
  }

  build() {
    Column() {
      Card({ title: '默认插槽' })
      Card({ title: '通过参数传递', component: this.componentBuilder, rightSlot: this.rightBuilder })
      // Card({ title: '会报错' }) {
      //   Text('直接传值').margin(30)
      // }
    }.height('100%').padding({ left: 16, right: 16, top: 20, bottom: 20 }).backgroundColor('#f0f3f6')
  }
}
```

效果图

![](https://cdn.nlark.com/yuque/0/2024/png/26122809/1719161176512-12969bbe-213a-42de-bf42-8ae93b961acb.png)

插槽的实现方式有很多，具体可以参考下官方文档[@BuilderParam装饰器：引用@Builder函数](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/arkts-builderparam-0000001524416541-V2)

## 参数必传
如果我想让，用我封装的组件，让父组件必须传递一个属性，可以使用Require修饰符

:::info
Require修饰符只能作用在两个修饰符前面Prop  BuilderParam

:::

**<font style="color:rgb(77, 77, 77);">自定义组件</font>**

```arkts

// 子组件
@Component
export struct Card {
  @Builder
  slot() {
    Text('暂无数据').margin(30)
  };

  @Builder
  defaultRightSlot() {
    Text('详情')
  };

  private title: ResourceStr
  // 父组件必须得传，不传则报错
  @Require
  @BuilderParam component: () => void = this.slot;
  @BuilderParam rightSlot: () => void = this.defaultRightSlot;

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

**<font style="color:rgb(77, 77, 77);">外部调用</font>**

```arkts
@Builder
function overBuilder() {
  Text('外部组件').margin(30)
}

// 父布局调用：
@Preview
@Component
@Entry
export struct Index {
  @Builder
  componentBuilder() {
    Text('内部组件').margin(30)
  }

  @Builder
  rightBuilder() {
    Text('右边的插槽').margin(30)
  }

  build() {
    Column() {
      // 没传component，会报错
      Card({ title: '通过参数传递', rightSlot: this.rightBuilder })
    }.height('100%').padding({ left: 16, right: 16, top: 20, bottom: 20 }).backgroundColor('#f0f3f6')
  }
}
```



插槽的实现方式有很多，具体可以参考下官方文档[@BuilderParam装饰器：引用@Builder函数](https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/arkts-builderparam-0000001524416541-V2)

