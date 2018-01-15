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

作为`headerTitle`的备选参数(译注：当`headerTitle`与`title`同时存在时，优先使用`headerTitle`)。另外，也会作为`tabBarLabel`(如果嵌套使用`TabNavigator`)和`drawerLabel`(如果嵌套使用`DrawerNavigator`)的备选参数。

`header`

React元素或者一个传入`HeaderProps`参数返回React元素的函数，用来渲染一个导航器。设置为`null`隐藏导航器。

`headerTitle`

字符串、React元素或React组件，供导航条使用。默认为场景的标题。当传入的是组件时，此组件接收`allowFontScaling`、`style`和`children`参数。标题字符串被透传给`children`。

`headerTitleAllowFontScaling`

标题标题字体是否应缩放以遵从文本大小辅助设置。默认为true。

`headerBackTitle`

iOS上返回按钮使用的标题字符串，设为`null`不显示返回标题。默认为前一个页面的`headerTitle`。

`headerTruncatedBackTitle`

当返回按钮的`headerBackTitle`超出屏幕时，将会使用此字符串。默认为"Back"。

`headerRight`

显示在导航栏右侧的React元素。

`headerLeft`

显示在导航栏左侧的React元素或者组件。当使用的是组件时，在该组件被渲染时，它将接收一系列参数(`onPress`、`title`、`titleStyle`以及其他参数 - 查看`Header.js`文件浏览完整的参数列表)

`headerStyle`

导航栏的样式对象

`headerTitleStyle`

标题组件的样式对象

`headerBackTitleStyle`

返回标题的样式对象

`headerTintColor`

导航栏的色调

`headerPressColorAndroid`

材质纹波的颜色（仅适用于Android> = 5.0）

`gesturesEnabled`

是否可以使用手势来滑动返回。在iOS上默认为true，在Android上为false。

`gestureResponseDistance`

该对象用来设置从屏幕边缘开始触摸的距离，以识别手势。它具有以下属性：
 - `horizontal` - number型，水平方向的距离。默认为25。
 - `vertical` - number型，垂直方向的距离。默认为135。

`gestureDirection`

字符串:用来覆写关闭手势的方向。`default`代表正常行为，`inverted`代表从右向左滑动。

### Navigator Props

使用`StackNavigator(...)`创建的导航器组件，带有一下参数：
 - `screenProps` - 将其他选项传递给子页面，例如：
 
```javascript
 const SomeStack = StackNavigator({
  // config
});

<SomeStack
  screenProps={/* this prop will get passed to the screen components as this.props.screenProps */}
/>
```
 
### 例子

请参阅示例[SimpleStack.js](https://github.com/react-community/react-navigation/tree/master/examples/NavigationPlayground/js/SimpleStack.js)和[ModalStack.js](https://github.com/react-community/react-navigation/tree/master/examples/NavigationPlayground/js/ModalStack.js)，你可以将其作为[NavigationPlayground](https://github.com/react-community/react-navigation/tree/master/examples/NavigationPlayground)应用程序的一部分在本地运行。

在你手机上访问[我们的epxo demo](https://exp.host/@react-navigation/NavigationPlayground)，你可以直接查看这些实例。

您可以通过访问我们的展会演示直接在您的手机上查看这些示例。

#### 自定义屏幕跳转的模态StackNavigator

```javascript
const ModalNavigator = StackNavigator(
 {
   Main: { screen: Main },
   Login: { screen: Login },
 },
 {
   headerMode: 'none',
   mode: 'modal',
   navigationOptions: {
     gesturesEnabled: false,
   },
   transitionConfig: () => ({
     transitionSpec: {
       duration: 300,
       easing: Easing.out(Easing.poly(4)),
       timing: Animated.timing,
     },
     screenInterpolator: sceneProps => {
       const { layout, position, scene } = sceneProps;
       const { index } = scene;

       const height = layout.initHeight;
       const translateY = position.interpolate({
         inputRange: [index - 1, index, index + 1],
         outputRange: [height, 0, 0],
       });

       const opacity = position.interpolate({
         inputRange: [index - 1, index - 0.99, index],
         outputRange: [0, 1, 1],
       });

       return { opacity, transform: [{ translateY }] };
     },
   }),
 }
);
```

导航栏的过渡也可以使用`transitionConfig`的属性`headerLeftInterpolator`、`headerTitleInterpolator`和`headerRightInterpolator`进行配置。
