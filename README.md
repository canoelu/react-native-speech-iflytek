# react-native-speech-iflytek
react-native-speech-iflytek 是一个 React Native 下的科大讯飞语音库，可以进行语音识别与语音合成。

## Support
- React Native >= 0.47.0 from 0.2.0
- React Native >= 0.42.0 from 0.1.2
- Android，目前仅支持 Android

## Install
```
npm i react-native-speech-iflytek --save
react-native link
```
安装后还须进行下面两步：
1. 在 [讯飞开放平台](http://www.xfyun.cn/sdk/dispatcher) 下载组合服务SDK（选择`语音听写`与`在线语音合成`）
2. 使用下载 SDK 的 `Android_voice_xxxx_xxxxxxxx/libs` 文件夹替换 `YourProject/node_modules/react-native-speech-iflytek/android/libs` 文件夹，这是因为讯飞语音的原生库与注册应用进行了绑定。

## Usage
（详见 Example）引入包：
```
import { Recognizer, Synthesizer, SpeechConstant } from "react-native-speech-iflytek";
```
语音识别：
```
Recognizer.init("57c7c5b0");
DeviceEventEmitter.addListener("onRecognizerResult", this.onRecognizerResult);
Recognizer.start();
```
处理识别结果：
```
onRecognizerResult(e) {
    if (!e.isLast) {
        return;
    }
    ToastAndroid.show(e.result, ToastAndroid.SHORT);
    this.setState({ text: e.result });
}
```
## API

### Recognizer
#### Methods
- `Recognizer.init(String AppId)`  
初始化语音识别
- `Recognizer.start()`  
开始语音识别
- `Recognizer.cancel()`  
取消语音识别
- `Recognizer.isListening()`  
检测当前是否正在语音识别。返回 `Promise`，结果为 `bool` 类型，表示当前是否正在语音识别
- `Recognizer.stop()`  
如果正在语音识别，则结束语音识别
- `Recognizer.setParameter(String parameter, String value)`  
语音识别设置，详见讯飞语音文档
- `Recognizer.getParameter(String param)`  
获取语音识别设置，详见讯飞语音文档。返回 `Promise`，结果为 `String` 类型，表示语音识别设置值
#### Events
- `onRecognizerResult(JSON result)`  
语音识别结果，在语音识别时会不断触发该事件，`result` 为 `JSON` 类型，其值：

    - `text`：当次识别结果
    - `result`：当前识别结果，最常使用
    - `isLast`：是否是最后一次识别，调用 `Recognizer.stop()` 后，`isLast` 值为 `true`，否则一直为 `false`
    - `duration`：当前识别时间长度
- `onRecognizerVolumeChanged(Int volume)`  
语音识别的音量大小，当识别的语音改变音量时会触发该事件
- `onRecognizerError(JSON error)`  
语音识别出现错误，错误信息与讯飞文档保持一致，其值：

    - `errorCode`: 获取错误码
    - `errorDescription`: 获取错误描述，不包含错误码的描述信息
    - `plainDescription`: 获取错误描述，包含错误码的描述信息

### Synthesizer
#### Methods
- `Synthesizer.init(String AppId)`  
初始化语音合成
- `Synthesizer.start(String content)`  
开始语音合成
- `Synthesizer.stop()`  
如果正在语音合成，则结束语音合成
- `Synthesizer.isSpeaking()`  
检测当前是否正在语音合成。返回 `Promise`，结果为 `bool` 类型，表示当前是否正在语音合成
- `Synthesizer.pause()`  
如果正在语音合成，则暂停语音合成
- `Synthesizer.setParameter(String parameter, String value)`  
语音合成设置，详见讯飞语音文档
- `Synthesizer.getParameter(String param)`  
获取语音合成设置，详见讯飞语音文档。返回 `Promise`，结果为 `String` 类型，表示语音合成设置值
#### Events
- `onSynthesizerBufferCompletedEvent()`  
语音合成缓冲完成时触发该事件
- `onSynthesizerSpeakCompletedEvent()`
语音合成播放完成时触发该事件

### SpeechConstant
本模块包含讯飞接口的所有常量，如设置发言人、发言速度等，详见讯飞文档，使用示例：
```
Synthesizer.setParameter(SpeechConstant.VOICE_NAME, "xiaoyu");
```

## Mayebe
- (Native Android) Android Studio 2.3.3 接入讯飞语音接口: http://www.jianshu.com/p/caf50402d31c

## Contribute
期待提出有关建议，欢迎做出贡献，感谢 star。  
Github: [https://github.com/zphhhhh/react-native-speech-iflytek](https://github.com/zphhhhh/react-native-speech-iflytek)
