## 一、集成 SDK

###### 1.使用远程依赖集成SDK：

```bash
 app build.gradle文件里添加：

    implementation'com.github.wujianneng:SwanPlayLib:1.1.2'

 项目根 build.gradle文件里添加：

    repositories {

       mavenCentral()

       maven { url 'https://jitpack.io' }

       maven {

           url 'http://4thline.org/m2'

       }

    }
```

###### 2.SDK初始化和设备搜索：

在需要展示设备列表的Activity或fragment的一些生命周期里，编写下列api

###### 在onCreate()或onCreateView()里：

```java
 //SWDeviceManager.getInstance().setGetInfoTask是用来开启三个定时任务的，

 //workPlayStatusTask是每1秒会执行一次的定时任务可以用来刷新时间等播放状态ui，

   //workMediaInfoTask也是每1秒会执行一次的定时任务，这里可以用来更新播放歌曲信息等ui，

 //workDeviceInfoTask是每5秒会执行一次的定时任务，这里可以用来更新设备列表的ui

  SWDeviceManager.getInstance().setGetInfoTask(SWDeviceManager.WorkPlayStatusTask workPlayStatusTask,SWDeviceManager

    .WorkMediaInfoTask workMediaInfoTask, SWDeviceManager.WorkDeviceInfoTask workDeviceInfoTask);



    SWDeviceManager.getInstance().init(getActivity());



    //SWDeviceManager.getInstance().getMasterDeviceList()是SDK维护的主设备列表，可以用来做UI展示

    mDevicesAdapter = new DevicesAdapter(mContext, SWDeviceManager.getInstance().getMasterDeviceList());

    mDeviceList.setAdapter(mDevicesAdapter);

    //设置主设备列表变化的监听，更新设备列表ui

    SWDeviceManager.getInstance().setOnMasterDeviceListChangeListener(() -> mDevicesAdapter.notifyDataSetChanged());
```

###### 在onResume()里：

```css
 SWDeviceManager.getInstance().pauseAllTask(false);
```

###### 在onPause()里：

```css
 SWDeviceManager.getInstance().pauseAllTask(true);
```

## 二、功能介绍

#### 1.设备配网：

###### **SoftAP 配网模式*

###### SWWiFiSetupManager

提供了Wi-Fi配网的能力。

###### LPApItem

路由信息：ssid名称，信号强度，bssid地址等，如下图

![](https://upload-images.jianshu.io/upload_images/30070782-a33f8f93632a526f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png

###### SWWiFiSetupFailed

errorCode 所对应的含义，如下图

![](https://upload-images.jianshu.io/upload_images/30070782-72e47f0cef8629ab.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png

###### 检测手机是否连接到设备热点

```undefined
  isLinkplayHotspot()
```

接口说明  

Wi-Fi配网模式需要手机的Wi-Fi连接到设备的热点网络上，所以需要检测手机的Wi-Fi是否连接到设备热点，可以设定检测的时间

###### 获取 Wi-Fi 列表

```undefined
  getApList（LPApListListener listener）
```

接口说明  

手机Wi-Fi连接到设备热点后，可以获取到周边的Wi-Fi列表。  

参数，如下图

![](https://upload-images.jianshu.io/upload_images/30070782-0721c3a8ccf52cfc.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png

###### 开始配网接口

```bash
  connectToWiFi(LPApItem item, String pwd, SWWiFiSetupListener listener)
```

接口说明  

设备连接Wi-Fi时，手机与设备之间的连接将会断开，手机会自动连接到其他Wi-Fi上。请提示用户将手机连接到目标Wi-Fi上。  

参数，如下图

![](https://upload-images.jianshu.io/upload_images/30070782-5d7966fbbeed1b58.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png

###### 重新检测配网结果

```cpp
  retryCheckWithTime（int timeout, SWWiFiSetupListener listener）
```

接口说明  

配网失败后，如果手机所连WiFi和选择的目标路由器Wi-Fi不一致，可调用此方法重新检测配网结果  

参数，如下图

![](https://upload-images.jianshu.io/upload_images/30070782-aabe70bf7ec63741.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

image.png

#### 2.设备管理：

###### 获取主设备列表

```css
  SWDeviceManager.getInstance().getMasterDeviceList();
```

###### 获取子设备列表

```java
  SWDeviceUtils.getSlaveList(String deviceip, SWDeviceUtils.GetSlaveListCallback callback);
```

###### 获取设备信息

```css
  SWDeviceUtils.getControlDeviceInfo(Device device, @Nullable ControlCallback callback)
```

#### 3.播放控制：

###### SWPlayControl

```cpp
  SWPlayControl mSWPlayControl = new SWPlayControl();
```

###### 播放多首歌曲

```css
  mSWPlayControl.playAudio(List<LPPlayItem> lpPlayItemList, @Nullable ControlCallback callback)
```

###### 播放一首歌曲

```css
  mSWPlayControl.playOneAudio(LPPlayItem lpPlayItem, @Nullable ControlCallback callback)
```

###### 播放当前播放列表里的某一首歌曲

```dart
  mSWPlayControl.playQueueWithIndex(int index, final LPDevicePlayerListener listener) 
```

###### 播放当前播放列表里的某一首歌曲

```dart
  mSWPlayControl.playQueueWithIndex(int index, final LPDevicePlayerListener listener) 
```

###### 删除当前播放歌单中的歌曲

```css
  mSWPlayControl.deleteTrackWithIndex(int position, @Nullable ControlCallback callback)
```

###### 获取当前播放列表

```java
  mSWPlayControl.getCurrentPlaylist(SWPlayControl.GetCurrentPlaylistCallback callback);
```

###### 播放

```css
  mSWPlayControl.play(final ControlCallback callback)
```

###### 暂停

```css
  mSWPlayControl.pause(final ControlCallback callback)
```

###### 停止

```css
  mSWPlayControl.stop(final ControlCallback callback)
```

###### 下一首

```css
  mSWPlayControl.next(final ControlCallback callback)
```

###### 上一首

```css
  mSWPlayControl.previous(final ControlCallback callback)
```

###### 设置播放进度

```dart
  mSWPlayControl.seek(int currentProgressPercent, final ControlCallback callback)
```

###### 设置主设备播放音量

```dart
  mSWPlayControl.setVolume(int currentVolumePercent, final ControlCallback callback)
```

###### 获取主设备播放音量

```css
  mSWPlayControl.getVolume(final ControlReceiveCallback callback)
```

###### 设置子设备播放音量

```dart
  mSWPlayControl.setSlaveDeviceVolume(String deviceIp, String slaveip, int volume, BaseCallback callback)
```

###### 设置播放是否静音

```java
  mSWPlayControl.setMute(boolean desiredMute, @Nullable final ControlCallback callback)
```

###### 设置主设备播放声道

```dart
  mSWPlayControl.setMasterDeviceChannel(String deviceIp, int channel, BaseCallback callback)
```

###### 设置子设备播放声道

```dart
  mSWPlayControl.setSlaveDeviceChannel(String deviceIp, String slaveip, int channel, BaseCallback callback)
```

###### 设置播放模式

```css
  mSWPlayControl.setPlayMode(int playMode, @Nullable ControlCallback callback)
```

###### 获取播放模式

```css
  mSWPlayControl.getPlayMode(final ControlReceiveCallback callback)
```

###### 获取播放模式

```css
  mSWPlayControl.getPlayMode(final ControlReceiveCallback callback)
```

###### 获取当前播放进度和媒体信息

```css
  mSWPlayControl.getPositionInfo(final ControlReceiveCallback callback)
```

#### 4.多设备同步播放音乐：

###### 同步多台设备

```css
  SWDeviceUtils.slaveListKicIn(Activity activity, SWDevice masterDevice, List<SelectSWDeviceBean> slaveList, BaseCallback callback)
```

###### 解绑多台设备

```css
 SWDeviceUtils.slaveListKicOut(Activity activity,SWDevice masterDevice, List<SelectSWDeviceBean> swDeviceList, BaseCallback callback)
```

#### 5.设备信息设置：

###### 设置设备名称

```dart
  SWDeviceUtils.setDeviceName(String deviceIp, String name, BaseCallback callback)
```

###### 恢复出厂设置

```css
  SWDeviceUtils.deviceRestoreToDefault(String masterIp, BaseCallback callback)
```

###### 设备网络检测

```css
  SWDevice.getSwDeviceInfo().getSWDeviceStatus().getInternet().equals("1")
```

## 作者

珠海惠威科技有限公司，[david.li@swanspeakers.com](mailto:david.li@swanspeakers.com)
