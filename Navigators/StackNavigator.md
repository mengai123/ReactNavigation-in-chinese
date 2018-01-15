# StackNavigator

为你的应用程序提供一种能在页面之间进行切换的方式，新的页面将会被放置在栈顶。

默认情况下，`StackNavigator`拥有与原生iOS和Android同样的外观和体验：在iOS上，新的页面从屏幕的右侧滑入；在Android上，从底部淡入。在iOS上，`StackNavigator`也可以配置为模态样式，即页面从屏幕底部滑入。

```javascript
class MyHomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Home',
  }

  render() {
    return (
      <Button
        onPress={() => this.props.navigation.navigate('Profile', {name: 'Lucy'})}
        title="Go to Lucy's profile"
      />
    );
  }
}

const ModalStack = StackNavigator({
  Home: {
    screen: MyHomeScreen,
  },
  Profile: {
    path: 'people/:name',
    screen: MyProfileScreen,
  },
});

```

## API说明

```javascript
StackNavigator(RouteConfigs, StackNavigatorConfig)
```

### RouteConfigs

路由配置对象是从路由名称到路由配置的映射，告诉`navigator`该路由渲染什么。

```javascript
StackNavigator({

  // 每个你可以跳转的页面，像这样创建新的入口
  Profile: {

    // `ProfileScreen` 是一个React组件，它将成为页面的主要内容
    screen: ProfileScreen,
    // 当`ProfileScreen`被StackNavigator加载时，它将会被传入一个`navigation`参数

    // 可选的：当深度链接或在web应用中使用react-navigation时，path参数将会使用：
    path: 'people/:name',
    // 动作和路由参数都可以从path路径中获取

    // 可选的：重写页面的`navigationOptions`
    navigationOptions: ({navigation}) => ({
      title: `${navigation.state.params.name}'s Profile'`,
    }),
  },

  // 其他路由：
  ...MyOtherRoutes,
});

```


### StackNavigatorConfig

路由选项：
- `initialRouteName` - 设置栈的默认页面。必须与路由配置中的某个键相匹配。
- `initialRouteParams` - 初始路由的参数。
- `navigationOptions` - 页面使用的默认导航选项。
- `paths` - 覆盖路由配置中的paths。

视觉选项：
* `mode` - 定义页面的渲染和切换风格。
    * `card` - 使用标准的iOS和Android屏幕切换。默认的。
    * `modal` - 使屏幕从底部滑入，在iOS上，这是一种常见的模式。只适用于iOS，对Android没有任何影响。
* `headerMode` - 指定导航条如何渲染：
    * `float` - 在页面顶部渲染一个单一的导航条，当页面更改时，有浮入动画。这是iOS上的常见模式。
    * `screen` - 每个页面都有一个依附的导航条，导航条与页面一起淡入淡出。这是Android上常见的模式。
    * `none` - 不渲染导航条

* `cardStyle` - 使用此参数去覆盖或扩展栈中单个卡片(页面)的默认样式。
* `transitionConfig` -  函数返回一个与页面默认跳转动画合并后的对象（查看[类型定义](https://github.com/react-community/react-navigation/blob/master/src/TypeDefinition.js)中的TransitionConfig）。提供的函数将传递以下参数：
    * `transitionProps` - 新页面的跳转参数。
    * `prevTransitionProps` - 旧页面的跳转参数。
    * `isModal` - 指定页面是否是模态形式。
* `onTransitionStart` - 跳转动画开始时，调用此函数。
* `onTransitionEnd` - 跳转动画完成时，调用此函数。

### Screen Navigation Options

`title`
作为`headerTitle`的一种备选
