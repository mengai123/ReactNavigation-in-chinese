# Quick Start Guide - 快速入门指南

要开始使用`React Navigation`，你只需通过`npm`安装`react-navigation`包即可。

*译者注：为了方便理解，下文中将所有`screen`翻译为`页面`*

### 使用npm安装

```
npm install --save react-navigation
```

### 使用Yarn安装

```
yarn add react-navigation
```

要开始使用`React Navigation`，你必须创建一个导航器。`React Navigation`带有三个默认导航器。

* `StackNavigator` - 为应用程序提供了一种在页面之间切换的方法，其中每个新页面都放置在堆栈的顶部。
* `TabNavigator` - 用于设置带有多个tab的页面。
* `DrawerNavigator` - 用于设置带抽屉导航的页面。


## 创建一个StackNavigator

`StackNavigator`是最常见的导航形式，所以我们将其用作基本演示。从创建一个`StackNavigator`开始。
```
import { StackNavigator } from 'react-navigation';

const RootNavigator = StackNavigator({

});

export default RootNavigator;
```

然后，我们可以为`StackNavigator`添加页面，每个键代表一个页面。
```
import React from 'react';
import { View, Text } from 'react-native';
import { StackNavigator } from 'react-navigation';

const HomeScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Home Screen</Text>
  </View>
);

const DetailsScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Details Screen</Text>
  </View>
);

const RootNavigator = StackNavigator({
  Home: {
    screen: HomeScreen,
  },
  Details: {
    screen: DetailsScreen,
  },
});

export default RootNavigator;
```

现在让我们为导航栏添加一个标题。
```
...

const RootNavigator = StackNavigator({
  Home: {
    screen: HomeScreen,
    navigationOptions: {
      headerTitle: 'Home',
    },
  },
  Details: {
    screen: DetailsScreen,
    navigationOptions: {
      headerTitle: 'Details',
    },
  },
});

export default RootNavigator;
```

最后，我们应该能从`Home`页面导航到`Details`页面。当你使用导航器注册一个组件时，组件将会添加一个`navigation`属性。这个`navigation`属性驱动如何在不同页面之间切换。

要从`Home`页面切换到`Details`页面，我们将要使用`navigation.navigate`，如下所示：
```
...
import { View, Text, Button } from 'react-native';

const HomeScreen = ({ navigation }) => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Home Screen</Text>
    <Button
      onPress={() => navigation.navigate('Details')}
      title="Go to details"
    />
  </View>
);

...
```

这样就可以了！这是使用`StackNavigator`和`React Navigation`的一个整体基础。下面是这个例子的完整代码：

```
import React from 'react';
import { View, Text, Button } from 'react-native';
import { StackNavigator } from 'react-navigation'; // 1.0.0-beta.14

const HomeScreen = ({ navigation }) => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Home Screen</Text>
    <Button
      onPress={() => navigation.navigate('Details')}
      title="Go to details"
    />
  </View>
);

const DetailsScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Details Screen</Text>
  </View>
);

const RootNavigator = StackNavigator({
  Home: {
    screen: HomeScreen,
    navigationOptions: {
      headerTitle: 'Home',
    },
  },
  Details: {
    screen: DetailsScreen,
    navigationOptions: {
      headerTitle: 'Details',
    },
  },
});

export default RootNavigator;
```

## 创建一个TabNavigator

要开始使用`TabNavigator`，首先要导入`TabNavigator`，并且创建一个新的`RootTabs`组件。
```
import { TabNavigator } from 'react-navigation';

const RootTabs = TabNavigator({

});

export default RootTabs;
```

然后，我们需要创建一些页面，并将其添加到我们的`TabNavigator`。
```
import React from 'react';
import { View, Text } from 'react-native';
import { TabNavigator } from 'react-navigation';

const HomeScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Home Screen</Text>
  </View>
);

const ProfileScreen = () => (
  <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
    <Text>Profile Screen</Text>
  </View>
);

const RootTabs = TabNavigator({
  Home: {
    screen: HomeScreen,
  },
  Profile: {
    screen: ProfileScreen,
  },
});

export default RootTabs;
```

快成功了！现在让我们来为标签栏设置一个标签和图标。
> 我们将在示例中使用[react-native-vector-icons](https://github.com/oblador/react-native-vector-icons)组件，如果你的项目中还没有安装这个组件，那么请安装它。

```
...
import Ionicons from 'react-native-vector-icons/Ionicons';

...

const RootTabs = TabNavigator({
  Home: {
    screen: HomeScreen,
    navigationOptions: {
      tabBarLabel: 'Home',
      tabBarIcon: ({ tintColor, focused }) => (
        <Ionicons
          name={focused ? 'ios-home' : 'ios-home-outline'}
          size={26}
          style={{ color: tintColor }}
        />
      ),
    },
  },
  Profile: {
    screen: ProfileScreen,
    navigationOptions: {
      tabBarLabel: 'Profile',
      tabBarIcon: ({ tintColor, focused }) => (
        <Ionicons
          name={focused ? 'ios-person' : 'ios-person-outline'}
          size={26}
          style={{ color: tintColor }}
        />
      ),
    },
  },
});

export default RootTabs;
```



