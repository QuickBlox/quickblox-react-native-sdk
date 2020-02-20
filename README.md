# QuickBlox React Native SDK

## Documentation
See docs at https://docs.quickblox.com/docs/react-native-quick-start

## Quick Start
This guide demonstarates how to connect **quickblox-react-native-sdk** to your project and start development.

### Create a new app in the Admin Panel
QuickBlox application includes everything that brings messaging right into your application - chat, video calling, users, push notifications, etc. To create a QuickBlox application, follow the steps below:

1. Register a new account following [this link](https://admin.quickblox.com/signup). Type in your email and password to sign in. You can also sign in with your Google or Github accounts. 
2. Create the app clicking **New app** button. 
3. Configure the app. Type in the information about your organization into corresponding fields and click **Add** button.
4. Go to **Dashboard** --> **Overview** --> **Credentials** groupbox and copy your **Application ID**, **Authorization Key**, **Authorization Secret**, and **Account Key**.

### Install React Native SDK into your app
To connect QuickBlox to your app just add it into your dependencies in `package.json` - to do so, in the root directory of the project execute the following command in terminal:

`npm install quickblox-react-native-sdk --save`

iOS and Android have different dependencies systems. For that reason, you need to install dependencies in your iOS project. Just locate **ios/** folder in the root directory of the project and enter the following command in terminal: 

`pod install`

### Send your first message
#### Initialize QuickBlox SDK

Initialize the framework with your application credentials. Pass `appId`, `authKey`, `authSecret`, `accountKey` to the `QB.settings.init` method using the code snippet below. As a result, your application details are stored in the server database and can be subsequently identified on the server. 

```javascript
const appSettings = {
  appId: '',
  authKey: '',
  authSecret: '',
  accountKey: '',
  apiEndpoint: '', // optional
  chatEndpoint: '' // optional
};

QB.settings
  .init(appSettings)
  .then(function () {
    // SDK initialized successfully
  })
  .catch(function (e) {
    // Some error occured, look at the exception message for more details
  });
```

#### Authorize user

In order to use the abilities of QuickBlox SDK, you need to authorize your app on the server, log in to your account and create a session. To get it all done call `QB.auth.login` method and pass `login` and `password` parameters to it using the code snippet below. 

```javascript
QB.auth
  .login({
    login: 'yourlogin',
    password: 'yourpassword'
  })
  .then(function (info) {
    // signed in successfully, handle info as necessary
    // info.user - user information
    // info.session - current session
  })
  .catch(function (e) {
    // handle error
  });
```

**Note!**
You must initialize SDK before calling any methods through the SDK except for the method initializing your QuickBlox instance. If you attempt to call a method without connecting, the error is returned.

#### Connect to chat

To connect to chat server, use the code snippet below:

```javascript
QB.chat
  .connect({
    userId: 12345,
    password: 'passw0rd!'
  })
  .then(function () {
    // connected successfully
  })
  .catch(function (e) {
    // some error occurred
  });
```

#### Create dialog

QuickBlox provides three types of dialogs: **1-1 dialog**, **group dialog**, and **public dialog**. Learn more about dialogs [here](/docs/react-native-messaging#section--dialogs-). 

Let’s create **1-1 dialog**. Call `QB.chat.createDialog` method and pass `QB.chat.DIALOG_TYPE.CHAT` parameter as a dialog type to it. `QB.chat.DIALOG_TYPE.CHAT` parameter allows specifying that two occupants are going to participate in the dialog.

```javascript
QB.chat
  .createDialog({
    type: QB.chat.DIALOG_TYPE.CHAT,
    occupantsIds: [12345]
  })
  .then(function (dialog) {
    // handle as necessary
  })
  .catch(function (e) {
    // handle error
  });
```

#### Receive messages

To receive new messages, assign event handler using the code snippet below:

```javascript
import { NativeEventEmitter } from 'react-native'

const emitter = new NativeEventEmitter(QB.chat)

emitter.addListener(QB.chat.EVENT_TYPE.MESSAGE.RECEIVED_NEW_MESSAGE, event => {
  const { type, payload } = event
  // type - type of the event (string)
  // payload - new message (object)
})
```

#### Send message

When a dialog is created, a user can send a message. To create and send your first message, call `QB.chat.sendMessage` method and specify the `dialogId` and `body` parameters to it. Pass `saveToHistory` parameter if you want this message to be saved in chat history that is stored forever.

```javascript
const message = {
  dialogId: 'dsfsd934329hjhkda98793j2',
  body: 'Hey there!',
  saveToHistory: true
};

QB.chat
  .sendMessage(message)
  .then(function () { /* send successfully */ })
  .catch(function (e) { /* handle error */ })
```

## LICENSE
For license information, please visit: https://quickblox.com/terms-of-use/
