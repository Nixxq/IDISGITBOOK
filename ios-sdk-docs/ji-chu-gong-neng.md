# 基础功能

## 日志输出设置

### 说明和用途

设置是否在console输出sdk的log信息。

#### 接口函数

```text
/** Set whether to output SDK log information in console.
 @param BFlag defaults NO (no log output); YES is set to output log information for debugging reference. NO must be set when publishing products..
 */
+ (void)setLogEnabled:(BOOL)bFlag;

```

#### 示例代码

```text
#import <UAT/HWUATSDKManager.h>
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Override point for customization after application launch.
    [HWUATSDKManager setLogEnabled:YES];
}

```

#### 效果演示

如果用户传入的AppKey为空的话，打印日志如下图：

![&#x63A7;&#x5236;&#x53F0;&#x65E5;&#x5FD7;&#x8F93;&#x51FA;](../.gitbook/assets/picture2.png)

## 自定义事件

### 说明和用途

自定义事件可以实现在应用程序中埋点来统计用户的点击行为。自定义事件目前包括“计数事件”和“计时事件”。

#### 注意事项

EventKey长度不超过64，若超过将截取0-64的部分上传。

#### 接口函数

#### 计数事件

```text
/**
 * Records event with given key.
 * @param key Event key
 */
+ (void)recordEvent:(NSString *)key;

/**
 * Records event with given key and count.
 * @param key Event key
 * @param count Count of event occurrences
 */
+ (void)recordEvent:(NSString *)key count:(NSUInteger)count;

/**
 * Records event with given key and duration.
 * @param key Event key
 * @param duration Duration of event in seconds
 */
+ (void)recordEvent:(NSString *)key duration:(NSTimeInterval)duration;

/**
 * Records event with given key and segmentation.
 * @param key Event key
 * @param segmentation Segmentation key-value pairs of event
 */
+ (void)recordEvent:(NSString *)key segmentation:(NSDictionary * _Nullable)segmentation;

/**
 * Records event with given key, segmentation and count.
 * @param key Event key
 * @param segmentation Segmentation key-value pairs of event
 * @param count Count of event occurrences
 */
+ (void)recordEvent:(NSString *)key segmentation:(NSDictionary * _Nullable)segmentation count:(NSUInteger)count;

/**
 * Records event with given key, segmentation, count, sum and duration.
 * @param key Event key
 * @param segmentation Segmentation key-value pairs of event
 * @param count Count of event occurrences
 * @param duration Duration of event in seconds
 */
+ (void)recordEvent:(NSString *)key segmentation:(NSDictionary * _Nullable)segmentation count:(NSUInteger)count duration:(NSTimeInterval)duration;

```

#### 示例代码

用霍e通iOS客户端一次采购了5800件商品

```text
[HWUATSDKManager recordEvent:@"Purchase_Product" segmentation:@{@"platform":@"iOS",@"quantity":@"5800"} count:1];
```

#### 计时事件

```text
/**
 * Starts a timed event with given key to be ended later. Duration of timed event will be calculated on ending.
 * @discussion Trying to start an event with already started key will have no effect.
 * @param key Event key
 */
+ (void)startEvent:(NSString *)key;

/**
 * Ends a previously started timed event with given key.
 * @discussion Trying to end an event with already ended (or not yet started) key will have no effect.
 * @param key Event key
 */
+ (void)endEvent:(NSString *)key;

/**
 * Ends a previously started timed event with given key, segmentation, count and sum.
 * @discussion Trying to end an event with already ended (or not yet started) key will have no effect.
 * @param key Event key
 * @param segmentation Segmentation key-value pairs of event
 * @param count Count of event occurrences
 */
+ (void)endEvent:(NSString *)key segmentation:(NSDictionary * _Nullable)segmentation count:(NSUInteger)count;
/**
 * Cancels a previously started timed event with given key.
 * @discussion Trying to cancel an event with already cancelled (or ended or not yet started) key will have no effect.
 * @param key Event key
 */
+ (void)cancelEvent:(NSString *)key;

```

注意：

`startEvent`需要和`endEvent`配对使用，需要传入相同的`eventKey`。 

`duration`参数传入的单位为秒\(s\)。

## 用户反馈

#### 注意事项

参数title和description不可以为空或长度为0，否则SDK会抛出相应错误并且此次反馈内容无效。

#### 接口函数

```text
/**
 * Records feedback.
 * @param title feedback title
 * @param description description of feedback
 */
+ (void)feedbackWithTitle:(NSString *)title description:(NSString *)description;
/**
 * Records feedback.
 * @param title feedback title
 * @param description description of feedback
 * @param userName User name of APP user
 * @param phone phone number name of APP user
 * @param email email address name of APP user
 */
+ (void)feedbackWithTitle:(NSString *)title description:(NSString *)description userName:(NSString * _Nullable)userName phone:(NSString *_Nullable)phone emailAddress:(NSString *_Nullable)email;

```

#### 代码示例

霍e通想调用接口统计用户反馈信息

```text
[HWUATSDKManager feedbackWithTitle:titleTxf.text description:desTxv.text userName:@"苏三" phone:@"17714182222" emailAddress:@"1234567@gmail.com"];
```

