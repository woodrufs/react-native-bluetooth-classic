
# react-native-bluetooth-classic

React Native Bluetooth Classic is meand to bridge the gap found with regards to IOS Bluetooth Classic using the External Accessory framework. 

Based off the [react-native-bluetooth-serial](https://github.com/rusel1989/react-native-bluetooth-serial) port, and updated to replace [CoreBluetooth](https://developer.apple.com/documentation/corebluetooth) (BLE) on IOS with [External Accessory](https://developer.apple.com/documentation/externalaccessory/eaaccessory).

## Versions

Since there seem to be some breaking changes introduced within React Native 0.60 and I'm not entirely sure how or if these changes will affect this projec; or that reason I feel it's important to start running with a number of release branches (for the time being) just in case things go down.  In the following table, the React Native version is the lowest version (from package.json).

| Version | React Native | Android | IOS | Confirmed |
| --- | --- | --- | --- | --- |
| 0.9.x | 0.59.9 | > 4.1 (16) | > IOS 9 | :white_check_mark: |
| 0.10.x | 0.60.0 | > 4.1 (16) | > IOS 9 | N/A |

## Getting started

### Install from NPM

```
$ npm install react-native-bluetooth-classic --save
```

React Native 0.60.0 [autolinking](https://github.com/react-native-community/cli/blob/master/docs/autolinking.md).

#### Local Install

Local installation can be used for contribution or customization using the instructions in [BluetoothClassicExample](./BluetoothClassicExample/README.md).

### Manual installation

Manual installation should really only need to be used for contribution, or if there are issues with autolinking that I'm not aware of.  Follow the old steps for manually linking.

#### IOS Manual

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `react-native-bluetooth-classic` and add `RNBluetoothClassic.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libRNBluetoothClassic.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Run your project (`Cmd+R`)<

#### IOS Pods

The development pod can be added updating the `Podfile` with the appropriate line:

```
pod 'react-native-bluetooth-classic', :path => '<PATH TO RNBluetoothClassic>'
```

#### Android

1. Open up `android/app/src/main/java/[...]/MainApplication.java`
  - Add `import kjd.reactnative.bluetooth.RNBluetoothClassicPackage;` to the imports at the top of the file
  - Add `new RNBluetoothClassicPackage()` to the list returned by the `getPackages()` method
2. Append the following lines to `android/settings.gradle`:
  	```
  	include ':react-native-bluetooth-classic'
  	project(':react-native-bluetooth-classic').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-bluetooth-classic/android')
  	```
3. Insert the following lines inside the dependencies block in `android/app/build.gradle`:
  	```
      implementation project(':react-native-bluetooth-classic')
  	```

#### Windows

Windows isn't added yet - it looks like with the latest `react-native init` there is no Windows by default.  I'm assuming this is due to the React Native windows project (future Ken's problem).

[Read it! :D](https://github.com/ReactWindows/react-native)

1. In Visual Studio add the `RNBluetoothClassic.sln` in `node_modules/react-native-bluetooth-classic/windows/RNBluetoothClassic.sln` folder to their solution, reference from their app.
2. Open up your `MainPage.cs` app
  - Add `using Bluetooth.Classic.RNBluetoothClassic;` to the usings at the top of the file
  - Add `new RNBluetoothClassicPackage()` to the `List<IReactPackage>` returned by the `Packages` method

## Contribute

When setting up the project for contribution follow all the usual Git contribution best practices.

#### Android

When first building the Android project there were issues with react-native-create-library and the version of Android/Gradle installed on my machine.  This needed to be resolved by ensuring that the project was inline with the version of Android Studio and the Android plugin for gradle.  In my case, the project was configured with 1.3.1 and 2.2, which caused problems, in order to resolve [Android plugin for gradle versions](https://developer.android.com/studio/releases/gradle-plugin.html)

1. Updated `gradle-wrapper.properties` to modify the line:
`distributionUrl=https\://services.gradle.org/distributions/gradle-5.1.1-all.zip`

2. Updated `build.gradle` to ensure the buildscript section matched the following:
  - Added google() to buildscript
  - added jcenter() to repositories

```
buildscript {
    repositories {
        google()
        jcenter()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:3.4.0'
    }
}

repositories {
    mavenCentral()
    google()
    jcenter()
}
```

#### IOS (xcode)

Much like Android, the IOS project was being edited through the BluetoothClassicExample project by opening files through the RNBluetoothClassic library folder.  Since React Native is a peer dependency (and annoying to download and install locally with the changes to NPM5) I just continued to do the development this way, it worked out since it allowed me to test the changes on the fly. 

Again - if someone can point me on how to resolve these issues easily, I'd love to get it sorted.

## License

The MIT License (MIT) - see full [license file](LICENSE)

## Example

The BluetoothClassicExample is included within the ./BluetoothClassicExample React Native project.

## Usage

Import the module using the following:

```javascript
import RNBluetoothClassic, { BTEvents } from 'react-native-bluetooth-classic';
```

In all cases the following API/Events are available within Javascript for both Android and IOS (no code splitting) if there are any native calls that are not available on the native side, the promise will be rejected with an appropriate message (kind of like UnssupportedOperationException since I'm used to Java) - I found this important as I see no point in duplicating code as the whole purpose of React Native was for me not to.

## API

The following API is available on both Android and IOS (unless specifically stated not).  I've done my best to duplciate all the methods available on both, so there should be no need to use Platform or file switching.  Each call will return a Promise - for any API calls that aren't supported on a specific environment, they should reject the promise with an 'UnsupportedOperation' error.



### Code Examples

#### requestEnabled(): Promise

TODO

#### isEnabled(): Promise

```javascript
let enabled = await RNBluetoothClassic.isEnabled();
console.log(`Bluetooth enabled? ${enabled}`)
```

#### list(): Promise

```javascript
let devices = await RNBluetoothClassic.list();
console.log(`Available devices: ${devices.length});
```

#### discoverDevices(): Promise

TODO

#### cancelDiscovery(): Promise

TODO

#### pairDevice(deviceId:String): Promise

TODO

#### unpairDevice(deviceId:String): Promise

TODO

#### connect(deviceId:String): Promise

```javascript
try {
  let connectedDevice = await RNBluetoothClassic.connect(device.id);
  this.setState({connectedDevice});
} catch (error) {
  console.log(error.message);
} 
```

#### disconnect(): promise

```javascript
await RNBluetoothClassic.disconnect();
this.setState({connectedDevice: undefined})
```

#### isConnected(): Promise

```javascript
let connectedDevice = await RNBluetoothClassic.isConnected();
if (connectedDevice) let device = RNBluetoothClassic.getConnectedDevice();
else console.log(`Not currently connected to a device`);
```

#### getConnectedDevice(): Promise

```javascript
let connectedDevice = await RNBluetoothClassic.getConnectedDevice();
if (connectedDevice) console.log(`Currently connected to ${connectedDevice.address}`);
else console.log(`Not currently connected to a device`);
```

#### write(message: String): Promise

```javascript
let message = this.state.text + '\r';   // Commands should end with \r
await RNBluetoothClassic.write(message);
```

#### readFromDevice(): Promise

```javascript
// Reads all content in the buffer - regardless of delimiter
let message = await RNBluetoothClassic.readFromDevice();
```

#### readUntilDelimiter(): Promise

```javascript
// Delimiter defaults to '\n' without setting manually
let message = await RNBluetoothClassic.readUntilDelimiter();
```

#### readUntilDelimiter(delimiter:String): Promise

```javascript
let message = await RNBluetoothClassic.readUntilDelimiter('~');
```

#### setDelimiter(String delimiter): Promise

```javascript
await RNBluetoothClassic.setDelimiter('~');
let message = RNBluetoothClassic.readUntilDelimiter();
```

#### available(): Promise

```javascript
let available = await RNBluetoothClassic.available();
if (available)
  let message = await RNBluetoothClassic.readFromDevice();  // All content or .readUntilDelimiter()
```

## Events
  
Attaching (and disconnecting) from events can be completed in the `componentWillMount` (`componentWillUnmount` respectively) using the following:

```javascript
componentWillMount() {     
  this.onRead = RNBluetoothClassic.addListener(BTEvents.READ, this.handleRead, this);
}

componentWillUnmount() {
  this.onRead.remove();
}
```
