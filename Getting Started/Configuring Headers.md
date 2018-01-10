# Configuring Headers - 配置导航条


> 译注：下文中将`Header`统一翻译为`导航条`。

导航条仅适用于`StackNavigator`。

在前面的例子中，我们在应用里创建了一个`StackNavigator`来显示几个页面。

当跳转到`Chat`页面时，可以在`navigate`函数中为新`route`传入指定参数。因此，我们可以为`Chat`页面传递聊天对象的名字。

```javascript
this.props.navigation.navigate('Chat', { user:  'Lucy' });
```

这个`user`参数可以在`Chat`页面被获取：

```javascript
class ChatScreen extends React.Component {
  render() {
    const { params } = this.props.navigation.state;
    return <Text>Chat with {params.user}</Text>;
  }
}
```

## 设置导航条标题

下一步，使用页面的参数来设置导航条的标题：

```javascript
class ChatScreen extends React.Component {
  static navigationOptions = ({ navigation }) => ({
    title: `Chat with ${navigation.state.params.user}`,
  });
  ...
}
```

<br>

<div>
    <img src="https://reactnavigation.org/assets/examples/basic-header-android.png" width="40%" height="40%">
    <img src="https://reactnavigation.org/assets/examples/basic-header-iphone.png" width="40%" height="40%">   
</div>

<br>


## 添加一个右侧按钮

接下来，我们添加一个`navigationOptions`对象，可以为`header`添加一个自定义的右侧按钮：

```javascript
static navigationOptions = {
  headerRight: <Button title="Info" />,
  ...
```

<br>

<div>
    <img src="https://reactnavigation.org/assets/examples/header-button-android.png" width="40%" height="40%">
    <img src="https://reactnavigation.org/assets/examples/header-button-iphone.png" width="40%" height="40%">   
</div>

<br>

可以使用`navigation prop`来定义`navigationOptions`。让我们来根据`route`的参数渲染不同的按钮，并设置当按钮按下时调用`navigation.setParams`函数。

```javascript
static navigationOptions = ({ navigation }) => {
  const { state, setParams } = navigation;
  const isInfo = state.params.mode === 'info';
  const { user } = state.params;
  return {
    title: isInfo ? `${user}'s Contact Info` : `Chat with ${state.params.user}`,
    headerRight: (
      <Button
        title={isInfo ? 'Done' : `${user}'s info`}
        onPress={() => setParams({ mode: isInfo ? 'none' : 'info' })}
      />
    ),
  };
};
```

现在，导航条可以与页面的`route`/`state`进行交互：

<br>

<div>
    <img src="https://reactnavigation.org/assets/examples/header-interaction-android.png" width="40%" height="40%">
    <img src="https://reactnavigation.org/assets/examples/header-interaction-iphone.png" width="40%" height="40%">   
</div>

<br>


## 导航条与页面组件进行交互

有时候，导航条需要访问页面组件的属性，例如函数或状态。

比如说，我们想创建一个“编辑联系人信息”的页面，在导航条上有一个保存按钮，我们希望当保存信息时，用一个`ActivityIndicator`来代替保存按钮。

```javascript
class EditInfoScreen extends React.Component {
  static navigationOptions = ({ navigation }) => {
    const { params = {} } = navigation.state;
    let headerRight = (
      <Button
        title="Save"
        onPress={params.handleSave ? params.handleSave : () => null}
      />
    );
    if (params.isSaving) {
      headerRight = <ActivityIndicator />;
    }
    return { headerRight };
  };

  state = {
    nickname: 'Lucy jacuzzi'
  }

  _handleSave = () => {
    // Update state, show ActivityIndicator
    this.props.navigation.setParams({ isSaving: true });
    
    // Fictional function to save information in a store somewhere
    saveInfo().then(() => {
      this.props.navigation.setParams({ isSaving: false});
    })
  }

  componentDidMount() {
    // We can only set the function after the component has been initialized
    this.props.navigation.setParams({ handleSave: this._handleSave });
  }

  render() {
    return (
      <TextInput
        onChangeText={(nickname) => this.setState({ nickname })}
        placeholder={'Nickname'}
        value={this.state.nickname}
      />
    );
  }
}
```

注：由于`handleSave`参数仅在组件安装时设置，因此不能立即在`navigationOptions`函数中使用。在设置`handleSave`之前，为了立即渲染并避免闪烁，我们给`Button`组件传递一个空的函数。
