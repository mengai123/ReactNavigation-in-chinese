# Hello Mobile Navigation - 你好，手机导航！

让我们使用`React Navigation`为Android和iOS构建一个简单的聊天类应用程序。

## 设置和安装

首先，在开始之前，确保你已经设置好了[React Native](https://reactnative.cn/docs/0.51/getting-started.html#content)，接下来，创建一个新的项目，并添加`react-navigation`:

```shell
# Create a new React Native App
react-native init SimpleApp
cd SimpleApp

# Install the latest version of react-navigation from npm
npm install --save react-navigation

# Run the new app
react-native run-android
# or:
react-native run-ios
```

如果你使用的是`create-react-native-app`，而不是`react-native init`:
```shell
# Create a new React Native App
create-react-native-app SimpleApp
cd SimpleApp

# Install the latest version of react-navigation from npm
npm install --save react-navigation

# Run the new app
npm start

# This will start a development server for you and print a QR code in your terminal.
```

确认你能成功看到在iOS/Android上运行的示例应用程序：

<div>
  <img src="https://reactnavigation.org/assets/examples/bare-project-android.png" width="40%" height="40%">
  <img src="https://reactnavigation.org/assets/examples/bare-project-iphone.png" width="40%" height="40%">
</div>

<br>
<br>

我们希望在iOS和Android上代码共享，所以删除`index.js`里的内容（如果你使用的是0.49之前的React Native，你需要删除`index.ios.js`和`index.android.js`里的内容），取而代之的是`import './App';`。此后，为了实现应用，需要创建新的文件`App.js`（如果你使用的是`create-react-native-app`命令创建的应用，这一步已经自动完成了）。

## 介绍`Stack Navigator`

我们打算在我们的app里使用`StackNavigator`，因为从概念上来说，我们想要获得一种“卡片式堆栈”的移动效果，也就是说把每个新的页面放到栈顶，返回时从栈顶移除。我们先从一个页面开始：
```javascript
import React from 'react';
import {
  AppRegistry,
  Text,
} from 'react-native';
import { StackNavigator } from 'react-navigation';

class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    return <Text>Hello, Navigation!</Text>;
  }
}

export const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen },
});

AppRegistry.registerComponent('SimpleApp', () => SimpleApp);
```

如果你是使用`create-react-native-app`命令创建的app，则自带的`App.js`文件将要被修改为：
```javascript
import React from 'react';
import { StyleSheet, Text, View } from 'react-native';
import { StackNavigator } from 'react-navigation';

class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome'
  };
  render() {
    return <Text>Hello, Navigation!</Text>;
  }
}

const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen }
});

export default class App extends React.Component {
  render() {
    return <SimpleApp />;
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center'
  }
});
```

界面的`title`是可以在静态对象`navigationOptions`中配置的，其中可以设置多个选项来配置导航器中页面的呈现方式。

现在iPhone和Android应用程序应该会显示相同的页面：
<br>

<div>
    <img src="https://reactnavigation.org/assets/examples/first-screen-android.png" width="40%" height="40%">
    <img src="https://reactnavigation.org/assets/examples/first-screen-iphone.png" width="40%" height="40%">   
</div>

<br>

## 添加一个新的页面

接下来，在我们的`App.js`文件中，添加一个新页面，叫做：`ChatScreen`，把它定义在`HomeScreen`的下面：
```javascript
// ...

class HomeScreen extends React.Component {
    //...
}

class ChatScreen extends React.Component {
  static navigationOptions = {
    title: 'Chat with Lucy',
  };
  render() {
    return (
      <View>
        <Text>Chat with Lucy</Text>
      </View>
    );
  }
}

```

然后，我们在`HomeScreen`组件中添加一个按钮，从而可以跳转到`ChatScreen`：跳转需要使用`navigate`提供的方法（`navigate`是界面的`navigation`的一个属性），通过给方法传入一个目标`routeName`进行跳转，在这里，我们的目标界面是`Chat`。
```javascript
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    const { navigate } = this.props.navigation;
    return (
      <View>
        <Text>Hello, Chat App!</Text>
        <Button
          onPress={() => navigate('Chat')}
          title="Chat with Lucy"
        />
      </View>
    );
  }
}
```
> （不要忘记从react-native导入View和Button: `import { AppRegistry, Text, View, Button } from 'react-native';`）

到此，任务还没有完成，我们还需要在`StackNavigator`中注册`Chat`，像下面这样：
```javascript
export const SimpleApp = StackNavigator({
  Home: { screen: HomeScreen },
  Chat: { screen: ChatScreen },
});
```

现在你可以导航到你的新页面，并且能够返回:

<br>

<div>
    <img src="https://reactnavigation.org/assets/examples/first-navigation-android.png" width="40%" height="40%">
    <img src="https://reactnavigation.org/assets/examples/first-navigation-iphone.png" width="40%" height="40%">   
</div>

<br>

## 传递参数

硬编码写死`ChatScreen`的标题方式有点不太好，如果我们能够传递一个名字来渲染，将会更有用，让我们来这样做。

除了在导航函数中指定目标`routeName`外，还可以向新路由传递参数。首先，我们来编辑`HomeScreen`组件，传递一个`user`给路由。

```javascript
class HomeScreen extends React.Component {
  static navigationOptions = {
    title: 'Welcome',
  };
  render() {
    const { navigate } = this.props.navigation;
    return (
      <View>
        <Text>Hello, Chat App!</Text>
        <Button
          onPress={() => navigate('Chat', { user: 'Lucy' })}
          title="Chat with Lucy"
        />
      </View>
    );
  }
}
```

然后我们可以编辑我们的`ChatScreen`组件，来显示通过路由传入的`user`参数：
```javascript
class ChatScreen extends React.Component {
  // Nav options can be defined as a function of the screen's props:
  static navigationOptions = ({ navigation }) => ({
    title: `Chat with ${navigation.state.params.user}`,
  });
  render() {
    // The screen's current route is passed in to `props.navigation.state`:
    const { params } = this.props.navigation.state;
    return (
      <View>
        <Text>Chat with {params.user}</Text>
      </View>
    );
  }
}
```
现在，当你导航到`Chat`页面时，你就可以看到上述的标题。尝试改变`HomeScreen`里的`user`参数，看看会发生什么！

<br>

<div>
    <img src="https://reactnavigation.org/assets/examples/first-navigation-android.png" width="40%" height="40%">
    <img src="https://reactnavigation.org/assets/examples/first-navigation-iphone.png" width="40%" height="40%">   
</div>

<br>
