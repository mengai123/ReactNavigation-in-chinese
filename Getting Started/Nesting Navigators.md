# Nesting Navigators - 导航器的嵌套

在移动应用程序中，组合形式的导航是很常见的。`React Navigation`中的路由和导航器可以组合在一起，可以让你为你的app定义复杂的导航结构。

对于我们的聊天应用程序，我们希望在第一个屏幕上放置几个标签，查看最近的聊天会话或所有联系人。

## `Tab Navigator`简介

首先，在`App.js`文件中新建一个`TabNavigator`：

```javascript
import { TabNavigator } from "react-navigation";

class RecentChatsScreen extends React.Component {
  render() {
    return <Text>List of recent chats</Text>
  }
}

class AllContactsScreen extends React.Component {
  render() {
    return <Text>List of all contacts</Text>
  }
}

const MainScreenNavigator = TabNavigator({
  Recent: { screen: RecentChatsScreen },
  All: { screen: AllContactsScreen },
});
```

如果`MainScreenNavigator`渲染为顶层的导航器组件，那它看上去会像这样：

<br>

<div>
    <img src="https://reactnavigation.org/assets/examples/simple-tabs-android.png" width="40%" height="40%">
    <img src="https://reactnavigation.org/assets/examples/simple-tabs-iphone.png" width="40%" height="40%">   
</div>

<br>

## 将导航器嵌套在页面中

我们希望这些tabs只在app的第一个页面中可见，当跳转新页面时，路由栈中的新页面可以覆盖这些tabs。

将带有tabs的导航器作为一个页面，添加到我们前面设置过的`StackNavigator`的顶部，

```javascript
const SimpleApp = StackNavigator({
  Home: { screen: MainScreenNavigator },
  Chat: { screen: ChatScreen },
});
```

因为`MainScreenNavigator`将被作为一个页面使用，所以我们可以给它设置`navigationOptions`属性。

```javascript
const SimpleApp = StackNavigator({
  Home: { 
    screen: MainScreenNavigator,
    navigationOptions: {
      title: 'My Chats',
    },
  },
  Chat: { screen: ChatScreen },
})
```

让我们也为每个tab添加一个按钮，都可以跳转到`Chat`页面：
```javascript
<Button
  onPress={() => this.props.navigation.navigate('Chat', { user: 'Lucy' })}
  title="Chat with Lucy"
/>
```
现在我们已经把一个导航器放在另一个导航器中，并且我们可以在导航器之间切换：

<br>

<div>
    <img src="https://reactnavigation.org/assets/examples/nested-android.png" width="40%" height="40%">
    <img src="https://reactnavigation.org/assets/examples/nested-iphone.png" width="40%" height="40%">   
</div>

<br>

## 在组件中嵌套导航器

有时需要对包含在组件中的导航器进行嵌套。这在导航器只占用屏幕一部分的情况下非常有用。为了将子导航器连接到导航树中，子导航器需要使用父导航器的`navigation`属性。
```javascript
const SimpleApp = StackNavigator({
  Home: { screen: NavigatorWrappingScreen },
  Chat: { screen: ChatScreen },
});
```

在这种情况下，`NavigatorWrappingScreen`不是一个导航器，它只是将导航器作为其渲染的一部分。
> 译注：`NavigatorWrappingScreen`代表组件，其中包含一个导航器。

```javascript
class NavigatorWrappingScreen extends React.Component {
  render() {
    return (
      <View>
        <SomeComponent/>
        <MainScreenNavigator/>
      </View>
    );
  }
}
```


如果此导航器渲染空白页面，就把




