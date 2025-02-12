# cocos游戏轻量级框架
[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg?style=flat-square)](https://996.icu)
* 以个人实践为主，某些处理可能并非是最佳实践。我刚入门游戏开发，还在不断学习中。
* 个人游戏开发博客，欢迎交流。https://www.yuque.com/fengyong/game-develop-road 。
* 重要更新
    * 2019-6-17，使用 dash 规范重命名文件。
    * 2019-1-30，更新项目名称为 cocos-game-framework，更新一些资源的命名方式

## 适用范围
* cocos-creator 版本：2.0.10。
* 适用于轻量级游戏，单一场景，界面使用 prefab 进行管理。
* 适用于新手，包含一些基础的游戏界面、逻辑、音频、本地存储、多语言管理。

## 使用方法
* 拷贝整个项目作为基础使用。
* 单文件直接使用，文件依赖参考文件头部import部分。针对3种特殊的依赖：
    1. G，在 g-global.ts 中找到对应的方法，拷贝。
    2. FMLog，使用 cc.log 代替，或者使用自定义的 log 方法。
    3. FMVersion，使用 true/false 代替，或者直接删除，或者使用自定义的版本判定方法。
* 思路参考，某些处理并非是最优解，但是提供了一个还算不错的思路可以参考。

## 文件说明
- [**`f-app`**] 游戏启动唯一主入口
- [**`f-global`**] 通用方法
- [**`f-local`**] 本地存储
- [**`fm/f-manager`**] 管理组
    - [**`fm-panel`**] 界面管理，界面 UI 动画管理
    - [**`fm-sound`**] 声音管理
    - [**`fm-i18n`**] 多语言管理
    - [**`fm-anima`**] 动画管理
    - [**`fm-log`**] log 信息管理
    - [**`fm-version`**] 版本管理，运行环境管理
    - [**`fm-http`**] 网络
- [**`ft/f-tools`**] 工具组，一般需要挂载到节点上
    - [**`ft-add-prefab`**] 在当前节点下添加一个 prefab
    - [**`ft-child`**] 添加 n 个子节点
    - [**`ft-null`**] 空脚本
    - [**`ft-size`**] 节点比例化修改大小
    - [**`ft-z-index`**] 编辑器中修改节点 zIndex
    - [**`ft-erase`**] 橡皮擦效果
    - [**`ft-color`**] 颜色控制工具
    - [**`ft-follow`**] 节点跟随工具
    - [**`ft-scroll-list`**] 滑动列表工具
    - [**`ft-modal`**] 子对话框组件
- [**`panel/MVC-VC`**] 界面组，脚本命名方式为 panel-*，需要挂载在界面的同名 prefab 下
    - [**`panel-loading`**] 开场时的载入页面
    - [**`panel-base`**] 标准页面（建议新建 panel 时直接复制 PanelBase.prefab 和 PanelBase.ts 并重命名）
    - [**`panel-wait`**] 一个通用的等待页面
    - [**`panel-guide`**] 一个个人使用的新手引导页面
    - [**`panel-message`**] 一个通用的消息页面
- [**`system/MVC-M`**] 子系统组，脚本命名方式为 s-*，游戏子系统管理
- [**`controller/MVC-VC`**] 控制器组，脚本命名方式为 c-*，游戏中重要组件的控制器
- [**`data`**] 数据文件,一般是 i18n 的语言脚本，color 的颜色脚本等

## 规范
### 代码命名规范
* 文件名采用 dash 规范，类名采用 PascalCase 规范。
* 常量采用“大写字母+下划线”命名。
* 变量、方法采用“小写字母+下划线”命名，本项是为了与引擎自带方法区分开，如果引擎是“小写字母+下划线”写的方法，则使用 camelCase。
### ts规范
* ts 定义的 enum/interface/type 采用 PascalCase 规范，并且开头第一个字母进行含义说明：类型 Type*，参数接口（不论是入参还是出参） Params*，数据 Data*。
* 优先使用 interface 和 type ，因为这两项是编译前定义的，编译后会丢掉；只在1种情况下使用 enum ，即在需要使用 cc.Enum 方法时；考虑 cocos-creator 对 namespace 的支持不好，也应当尽量避免使用，使用 class 代替。
* 在 class 中，建议全部使用 private 封装属性和方法，只对需要外部调用的属性和方法 public。
* 总是使用箭头函数代替 function，包括在描述类型时。
* 使用 export 而不是 export default
### 脚本规范
* 依次分别为：import，ccclass/C，enum/interface/type，other-class，main-class 
### 遍历（针对 Array 和 Map 类型，object 类型建议通过 Object.keys 方法转化为 Array 后遍历）
* 使用 forEach 方法遍历。
* 使用一些特定的内置方法使得遍历逻辑更加明显。参考：https://www.yuque.com/fengyong/game-develop-road/gwv8xo 。
* 大数组遍历时，使用普通 for 循环，并提前计算好 length，这样的性能是最高的。
* 在遍历过程中如果需要 break 和 return，使用普通 for 循环。

## 两层逻辑设计：M + VC，M 表示数据层 system，VC 显示/控制层 controller
1. 数据层 system
    * 实现游戏逻辑，处理游戏数据
    * 以子系统的方式区分逻辑和数据，例如：新手引导系统，离线奖励系统，分数系统
2. 显示层 panel/controller
    * panel 为单个界面逻辑
    * controller 为界面中重要组件逻辑
3. 合并与拆分
    * M 与 VC 的合并：如果某个系统只对应单一的 panel，则合并到 panel 中，如 panel-message
    * VC 的拆分：VC 仅仅在是文件上合并，在实际逻辑中依然是拆分的