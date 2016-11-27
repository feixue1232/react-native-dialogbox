[![npm version](https://img.shields.io/npm/v/react-native-dialogbox.svg?style=flat-square)](https://www.npmjs.com/package/react-native-dialogbox)
[![npm downloads](https://img.shields.io/npm/dm/react-native-dialogbox.svg?style=flat-square)](https://www.npmjs.com/package/react-native-dialogbox)
[![Dependencies](https://david-dm.org/victoriafrench/react-native-dialogbox.svg)](https://david-dm.org/victoriafrench/react-native-dialogbox)
[![React-Native](https://img.shields.io/badge/react--native-v0.38.0-green.svg)]()
[![platforms](https://img.shields.io/badge/platforms-ios%20%7C%20android-blue.svg)]()
# react-native-dialogbox

This is a custom component for React Native, a simple popup, compatible with ios and android.

>This is a forked distro of react-native-popup that adds support for the current versions of react-native. The originating distro can be found [here](https://github.com/beefe/react-native-popup)

###Demo
![ui](./ui.gif)

###Props
- <b>name</b> *string* - unique name to register component with DialogReferenceManager
- <b>isOverlay</b> *bool* - *`default true`*
- <b>isOverlayClickClose</b> *bool* - *`default true`*

###Methods
- <b>alert</b>(<b>`message`</b>: *string*|*number*, [...])
```javascript
	e.g.

		this.dialogbox.alert(1);

		this.dialogbox.alert(1, 'two', '10 messages at most');
```
- <b>tip</b>({ <b>`title`</b>: *string*, <b>`content`</b>: *string*|*number*|*array*<*string*|*number*> *`isRequired`*, <b>`btn`</b>: {<b>`title`</b>: *string* <b>*`default 'OK'`*</b>, <b>`callback`</b>: *function*}, })
```javascript
	e.g.

		this.dialogbox.tip({
			content: 'come on!',
		});

		this.dialogbox.tip({
			title: 'TipTip',
			content: 'come on!',
		});

		this.dialogbox.tip({
			content: ['come on!', 'go!'],
			btn: {
				text: 'OKOK',
				callback: () => {
					this.dialogbox.alert('over!');
				},
			},
		});
```
- <b>confirm</b>({ <b>`title`</b>: *string*, <b>`content`</b>: *string*|*number*|*array*<*string*|*number*> *`isRequired`*, <b>`ok`</b>: {<b>`title`</b>: *string* *`default 'OK'`*, <b>`callback`</b>: *function*}, <b>`cancel`</b>: {<b>`title`</b>: *string* *`default 'Cancel'`*, <b>`callback`</b>: *function*}, })
```javascript
	e.g.

		this.dialogbox.confirm({
			content: 'Are you ready?',
		});

		this.dialogbox.confirm({
			content: 'Are you ready?',
			ok: {
				callback: () => {
					this.dialogbox.alert('Very good!');
				},
			},
		});

		this.dialogbox.confirm({
			title: 'title',
			content: ['come on!', 'go!'],
			ok: {
				text: 'Y',
				callback: () => {
					this.dialogbox.alert('Good!');
				},
			},
			cancel: {
				text: 'N',
				callback: () => {
					this.dialogbox.alert('Hurry up！');
				},
			},
		});
```

###Usage
####Step 1 - install

```
	npm install react-native-dialogbox --save
```

####Step 2 - import and use in project

```javascript
import React, { Component } from 'react';
import { View, Text, StyleSheet } from 'react-native';
import DialogBox from 'react-native-dialogbox';

export default class App extends Component{

	handleOnPress = () => {
		// alert
		this.dialogbox.alert(1);
	},

	render() {
		return (
			<View style={styles.container}>

				<Text style={styles.btn} onPress={this.handleOnPress}>click me !</Text>

				{/** dialogbox component */}
				<DialogBox ref={(dialogbox) => { this.dialogbox = dialogbox }}/>

			</View>
		);
	},

};
```

###Example working with DialogReferenceManager

###Outer container
```javascript
import React, { Component } from 'react';
import { View, StyleSheet } from 'react-native';
import DialogBox from 'react-native-dialogbox';

export default class App extends Component {
	render() {
		return (
			<View style={styles.app}>
				<MyComponent />
				<DialogBox name="default" />
			</View>
		);
	}
}
```

###Sub component
```javascript
import React, { Component } from 'react';
import { View, StyleSheet } from 'react-native';
import { DialogReferenceManager } from 'react-native-dialogbox';

export default class MyComponent extends Component {
	handleOnPress = () => {
		DialogReferenceManager.dialog('default').alert(1);
	}

	render() {
		return (
			<View style={styles.container}>
				<Text style={styles.btn} onPress={this.handleOnPress}>click me !</Text>
			</View>
		)
	}
}
```

DialogReferenceManager allows you to access instances of the DialogBox that exist elsewhere in the render chain. It requires that the name property be set on the DialogBox. Shortcut handlers for `alert`, `tip`, `confirm`, and `close` are available for the component registered as `default` as such:

```javascript
DialogReferenceManager.alert(1); // only works if a component exists named 'default'
DialogReferenceManager.dialog('AnyName').alert(1); // allows to access any named dialogbox
```

This feature was implemented due to the way that absolute positioning works. Users wanted to call a dialogbox in code, but it required the component to be registered at the top of the render chain.
