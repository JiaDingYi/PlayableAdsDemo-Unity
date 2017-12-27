
## PlayableAdsSDK for Unity
  1. 概述
  2. Demo简介
  3. 导入PlayableAds.unitypackage
  4. 安装可玩SDK

## 概述
    1. 面向人人群，本产品主要面向需要在Unity产品中接入可玩广告SDK的开发者
    2. 开发环境配置
        - iOS:
            Xcode 7.0或更高版本
            iOS 8.0或更高版本
        - Android:
            Android 4.0或更高版本
    3. 示例所使用的环境
        - iOS
            mac：macOS Sierra 10.12.6
            unity: version 2017.1.1f1 Personal
            xcode: Version 9.1 (9B55)
            cocoapods: 1.2.1
        - Andoird
            mac：macOS Sierra 10.12.6
            unity: version 2017.1.1f1 Personal

## Demo简介
下载Demo源码后导入到Unity中，打开场景SampleScene.unity

### 1. 主体界面及基本配制，如下所示
![image](/images/image01.png)
### 2. Main Camera的控件文件如下
![image](/images/image02.png)

本示例以Main Camera为广告事件接收对象，可以使用其它GameObject作为可玩广告事件接收对象，但要保证请求广告方法与事件接收所处于同一个GameObject下。

使用多个GameObject请求广告时，只以最后一个GameObject为准。

## 导入PlayableAds.unitypackage

### 1. 导入可玩插件包
Assets->Import Package -> Custom Package...
![image](/images/image03.png)
PlayableAds.unitypackage[资源位置](/PlayableAds.unitypackage)

### 2. 选择下载好的PlayableAds插件包，双击打开
![image](/images/image04.png)

### 3. 按需导入相关文件
a. 导入全部文件，其中包括iOS插件与Android插件，如图：
![image](/images/image05.png)

b. 只导入iOS插件，如图：
![image](/images/image20.png)

c. 只导入Android插件，如图：
![image](/images/image21.png)

如果出现文件名称冲突，请手动修改文件名与类名，只要确保调用时一致就可以。

### 4. 插件API说明
#### a. iOS插件API说明
 - 请求广告
    ``` c#
    // APP_ID为你在ZPLAY Ads平台申请的应用ID
    // AD_UNIT_ID为你在ZPLAY Ads平台申请的广告位ID
    PlayableAdsBridge.RequestAd(gameObjectName, APP_ID, AD_UNIT_ID);
    ```
 - 判断广告是否加载完成
    ``` c#
    PlayableAdsBridge.IsReady();
    ```
 - 展示广告
    ``` c#
    PlayableAdsBridge.PresentAd();
    ```
 - 自定义事件
    ```c#
    // 位置：Demo/Assets/Scripts/PlayableAdsBridge.IPlayableListener
    interface IPlayableListener{

        // 广告已经加载完毕，可以展示广告了
        void PlayableAdsDidLoad(string msg)

        // 广告加载失败，可根据error信息查找原因
        void DidFailToLoadWithError(string error)

        // 此时，用户已经看完整个广告了，可以下发奖励
        void PlayableAdsDidRewardUser(string msg)

        // 可玩SDK其它回调信息，详情查看msg
        void PlayableAdFeedBack(string msg)
    }
    ```

#### b. Android插件API说明
 - 初始化SDK
    ``` c#
    // APP_ID为你在ZPLAY Ads平台申请的应用ID
    PlayableAdsAdapter.Init(gameObjectName, APP_ID);
    ```
 - 请求广告
    ``` c#
    PlayableAdsAdapter.RequestAd(AD_UNIT_ID);
    ```
 - 判断广告是否加载完成
    ``` c#
    PlayableAdsAdapter.IsReady(AD_UNIT_ID)
    ```
 - 展示广告
    ``` c#
    PlayableAdsAdapter.PresentAd(AD_UNIT_ID)
    ```
 - 自定义事件
    ``` c#
    // 位置：Demo/Assets/Scripts/PlayableAdsAdapter.IPlayableAdapterListener
    interface IPlayableAdapterListener{

        // 广告已经加载完毕，可以调用PresentAd展示广告了
        void OnLoadFinished(string msg);

        // 广告加载失败，可根据error信息查找原因
        void OnLoadFailed(string error);

        // 此时，用户已经看完整个广告了，可以下发奖励
        void PlayableAdsIncentive(string msg);

        // 可玩SDK其它回调信息，详情查看msg
        void PlayableAdsMessage(string msg);
    }
    ```
**注意事项**：

1.自定义事件要与广告请求时的GameObject同属一个，如本示例的GameObject均为Main Camera。

2.发布应用时不要使用Demo中的应用id和广告位id，Demo中的id为测试id，收益为0。

## 安装可玩SDK
以下为iOS 可玩SDK的安装步骤，Android可玩SDK已经包含在插件包里了，不需要额外安装。
### 1. 进入Unity导出的xcode项目根目录下，初始化pod，如示例中的iOSProj目录
![image](/images/image14.png)
### 2. 初始化pod后会生成Podfile文件，在此文件下添加可玩sdk，如下
![image](/images/image15.png)
根据项目的不同，这个文件可能有所不同，只要确保将```pod 'PlayableAds'```添加到Podfile中即可。
注意：可玩广告SDK最低支持ios8.0，

### 3. 安装可玩sdk
```
pod install --repo-update
```
![image](/images/image16.png)
看到红线圈出的部分代表可玩广告SDK安装成功，此时可以运行项目查看运行效果了，步骤如下
### 4. 验证SDK是否安装成功
双击打开.xcworkspace文件，在xcode中安装应用到iPhone
![image](/images/image17.png)
注意：此处打开的是 **.xcworkspace** 文件，而非.xcodeproj
### 5. 预览demo
完整流程是点击“Request”开始请求广告，广告加载完成后提示“PlayableAdsDidLoad”，此时点击“Present”展示广告，广告展示完成后，点击“X”关闭广告，此时接收到“PlayableAdsDidRewardUser”消息。
![image](/images/image18.jpg)

如有疑问，请参考示例程序或发送邮件至liguodong@zplay.cn