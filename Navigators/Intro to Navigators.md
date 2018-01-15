# Intro to Navigators - 导航器简介

导航器允许定义应用程序的导航结构。导航器还可以渲染一些通用元素，比如可以配置它的标题栏和选项卡(tabs)。

在组件底层，导航器是普通的React组件。

## 内置的导航器

`react-navigation`包括以下功能来帮助您创建导航器:

- [StackNavigator](/StackNavigator.md) - 一次渲染一个页面，并提供页面之间的跳转。当一个新的页面被打开时，它会被放置在栈顶。

- [TabNavigator](/TabNavigator.md) - 渲染一个标签栏，让用户在多个页面之间切换。

- [DrawerNavigator](/DrawerNavigator.md) - 提供从屏幕左侧滑入的抽屉。

## 用导航器渲染页面

导航器渲染的应用程序页面，仅仅是React组件。

要了解如何创建屏幕，请阅读以下内容：

- [Screen navigation prop](/The%20Navigation%20Prop.md) 允许页面调度导航操作，比如打开一个新的页面。

- [Screen navigationOptions](/Screen%20Navigation%20Options.md) 自定义导航器的页面如何渲染（如导航条的标题，tab文本等）。

## 在顶层组件中调用导航器

如果你想在声明`Navigator`的同一层使用`Navigator`，你可以用react的`ref`选项:

```javascript
import { NavigationActions } from 'react-navigation';

const AppNavigator = StackNavigator(SomeAppRouteConfigs);

class App extends React.Component {
  someEvent() {
    // call navigate for AppNavigator here:
    this.navigator && this.navigator.dispatch(
      NavigationActions.navigate({ routeName: someRouteName })
    );
  }
  render() {
    return (
      <AppNavigator ref={nav => { this.navigator = nav; }} />
    );
  }
}
```

请注意，这个解决方案只能用在顶级导航器上。

## 导航容器

The built in navigators can automatically behave like top-level navigators when the navigation prop is missing. This functionality provides a transparent navigation container, which is where the top-level navigation prop comes from.

When rendering one of the included navigators, the navigation prop is optional. When it is missing, the container steps in and manages its own navigation state. It also handles URLs, external linking, and Android back button integration.

For the purpose of convenience, the built-in navigators have this ability because behind the scenes they use createNavigationContainer. Usually, navigators require a navigation prop in order to function.

顶层的导航器接收下面的参数：
```javascript
onNavigationStateChange(prevState, newState, action)
```

每当被`navigator`管理的导航器的状态变化时，都会调用上面的函数。它接收导航的历史状态、新状态以及造成状态改变的动作。默认情况下，它将状态更改打印到控制台。

```javascript
uriPrefix
```

应用程序可处理的URI的前缀。当处理深度链接，提取路径传递给路由器时，将使用此选项。
