
# circles-ui

> 仿Vant-ui移动端Vue UI组件库(circle star miss)。[文档地址](http://116.62.141.204:888/#/)

# 快速上手

## 安装
> 在Vue项目根目录下

```bash
npm i circles-ui --save-dev
```

## 全局引入
> 在main.js里新增以下代码

 ```bash
 import csUI from 'circles-ui'
 import 'circles-ui/packages/theme-default/lib/index.min.css'
 Vue.use(csUI)
 ```

## 按需引入
- 首先，安装 babel-plugin-component：

```shell
cnpm i babel-plugin-component --save-dev
```

* 更改根目录的.babelrc文件为下面的代码
```
{
  "presets": [
    ["env", {
      "modules": false,
      "targets": {
        "browsers": ["> 1%", "last 2 versions", "not ie <= 8"]
      }
    }],
    "stage-2"
  ],
  "plugins": ["transform-vue-jsx", "transform-runtime", ["component", {
    "libraryName": "circles-ui",
    "styleLibrary": {
      "name": "theme",
      "base": false
    }
  }]]
}
```
* 在需要引入的Vue页面新增以下代码

```bash
import { csPicker } from 'circles-ui'
import Vue from 'vue'
Vue.use(csPicker)
```

# 文档

## 主题色
> 2020.09.07新增主题色，使用css variable实现，请注意兼容性。要使用主题色只需在注册csUI时修改为Vue.use(csUI, themeName)，目前支持的主题色有

| 主色(默认) | 成功 | 警告 | 危险 | 详情 |
|--|--|--|--|--|
| Primary | Success | Warning | Danger | Info |
| #1989fa | #69c23a | #E6A23C | #FF2C7D | #909399 |

代码示例:
```
Vue.use(csUI, 'Warning')
```


## 基础组件

[`Icon` Icon图标](./docs/cn/icon.md)

[`Popup` 弹出层组件](./docs/cn/popup.md)

[`Picker` 选择器组件](./docs/cn/picker.md)

[`Toast` 弱提示组件](./docs/cn/toast.md)

[`Dialog` 弹出框组件](./docs/cn/dialog.md)

[`DateTimePicker` 时间选择器组件](./docs/cn/dateTimePicker.md)

[`ChartRadar` 雷达图组件](./docs/cn/chartRadar.md)

[`ActionSheet` 动作面板组件](./docs/cn/actionSheet.md)

[`Swipe` 轮播组件](./docs/cn/swipe.md)

[`SwipeCell` 滑动单元格组件](./docs/cn/swipeCell.md)

[`NumberKeyboard` 数字键盘组件](./docs/cn/numberKeyboard.md)

[`PasswordInput` 密码输入框组件](./docs/cn/passwordInput.md)

[`Collapse` 折叠面板组件](./docs/cn/collapse.md)

[`Switch` 开关组件](./docs/cn/switch.md)

[`Radio` 单选组件](./docs/cn/radio.md)

[`Stepper` 步进器组件](./docs/cn/stepper.md)

[`Checkbox` 复选框组件](./docs/cn/checkbox.md)

[`NoticeBar` 通知栏组件](./docs/cn/noticeBar.md)

[`Tab` 标签页组件](./docs/cn/tab.md)

[`Tabbar` 标签栏组件](./docs/cn/tabbar.md)

[`IndexBar` 索引栏组件](./docs/cn/indexBar.md)

[`Lazy` 懒加载组件](./docs/cn/lazy.md)

[`Circle` 环形进度条](./docs/cn/circle.md)

[`Skeleton` 骨架屏](./docs/cn/skeleton.md)


