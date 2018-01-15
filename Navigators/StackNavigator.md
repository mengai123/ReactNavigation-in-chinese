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




