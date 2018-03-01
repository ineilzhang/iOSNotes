####1.http 请求添加 info.plist 节点

```html
<dict>
	...
	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>
	...
</dict>
```

####2.touchesBegan 方法没有调用

 加载 View 时，设定背景颜色就 OK 了？？？
  
####3.git 添加本地代码库到远程代码库

```git remote add <name> <url>```

####4.git 建立本地分支的远程追踪分支

```git branch --set-upstream-to=<remote>/<branch> master```

####5.git 建立远程上传流

```git push --set-upstream origin animation```

####6.print 打印格式

|  格式 | 作用 |
| ---- | --- |
|%03d  | 输出三位宽度的整数， 不足时前补0|
|%3d   | 输出三位宽度的整数， 不足时后补空格|
|%-3d  | 输出三位宽度的整数， 不足时前补空格|
|%d    | 输出整数 未指定宽度，以实际宽度输出|

####7.静态全局变量

iOS 中 staic 和 const 常用使用场景，是用来代替宏，把一个经常使用的字符串常量，定义成静态全局只读变量.

```objective-c
// 开发中经常拿到 key 修改值，因此用 const 修饰key,表示key只读，不允许修改。
static  NSString * const key = @"name";

// 如果 const 修饰 *key,表示 *key 只读，key 还是能改变。

static  NSString const * key = @"name";
```

####8. weak 和 strong

```objective-c
#define STRONGSELF typeof(weakSelf) __strong strongSelf = weakSelf
#define WEAKSELF typeof(self) __weak weakSelf = self
if (server.status == TSSTBSoapConnectStatusConnected) {
    WEAKSELF
    [server hpstb_disconnect:^(NSError *error, id data){
      if (error) {
        STRONGSELF
        [strongSelf handleSetOrGetFailCallback:error callback:callback];
      } else {
        if (callback) {
          callback(@[kGSOAPEvent2000,kGSOAPEvent2000MSG]);
        }
      }
    }];
  }
```

####9. git stash 

- git stash ( pop | apply ) [--index] [-q|--quiet] [<stash>]
- git stash clear

####10. SCP 

[SCP 相关命令](http://see.sl088.com/wiki/Shell_SCP/%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6)


####11. @Protocol 的定义问题

```objective-c
//一定要实现<NSObject>协议
@protocol DownloadImageDelegate <NSObject>

- (void)downloadImage:(UIImage *)image;

@end
```

####12. UIView的各种常用属性

- **clipsToBounds**  

>A Boolean value that determines whether subviews are confined to the bounds of the view.  
>Setting this value to YES causes subviews to be clipped to the bounds of the receiver. If set to NO, subviews whose frames extend beyond the visible bounds of the receiver are not clipped. The default value is NO.

- **contentMode**

> A flag used to determine how a view lays out its content when its bounds change.


```objective-c
typedef NS_ENUM(NSInteger, UIViewContentMode) {
    UIViewContentModeScaleToFill,
    UIViewContentModeScaleAspectFit,      // contents scaled to fit with fixed aspect. remainder is transparent
    UIViewContentModeScaleAspectFill,     // contents scaled to fill with fixed aspect. some portion of content may be clipped.
    UIViewContentModeRedraw,              // redraw on bounds change (calls -setNeedsDisplay)
    UIViewContentModeCenter,              // contents remain same size. positioned adjusted.
    UIViewContentModeTop,
    UIViewContentModeBottom,
    UIViewContentModeLeft,
    UIViewContentModeRight,
    UIViewContentModeTopLeft,
    UIViewContentModeTopRight,
    UIViewContentModeBottomLeft,
    UIViewContentModeBottomRight,
};
```

####13. enumeration的定义

```objective-c
typedef NS_ENUM(NSInteger,SDImageFormat){
	SDImageFormatUndefined = -1,
	SDImageFormatPNG = 0,
	SDImageFormatJPEG,
	SDImageFormarGIF,
	SDImageFormatWebP,
	SDImageFormatTIFF
}
```

####14. 判断文件格式

```objective-c
//命名规范 sd_imageFormatForImageData
//参数可为空 nullbale 不可为空 nonnull
+(SDImageFormat)sd_imageFormatForImageData:(nullable NSData *)imageData
{
	//空参检测
    if (!imageData) {
        return SDImageFormatUndefined;
    }
    //typedef unsign char uint8_t 
    uint8_t firstByte;
    //取文件头判断图片文件类型
    [imageData getBytes:&firstByte length:1];
    switch (firstByte) {
        case 0x89:
            return SDImageFormatPNG;
        case 0xFF:
            return SDImageFormatJPEG;
        case 0x47:
            return SDImageFormatGIF;
        case 0x49:
        case 0x4D:
            return SDImageFormatTIFF;
        //0x52 可能是rar类型文件
        case 0x52:
            if (imageData.length < 12) {
                return SDImageFormatUndefined;
            }
            NSString *fileHead = [[NSString alloc]initWithData:[imageData subdataWithRange:NSMakeRange(0, 12)] encoding:NSASCIIStringEncoding];
            if ([fileHead hasPrefix:@"RIFF"] && [fileHead hasSuffix:@"WEBP"]) {
                return SDImageFormatWebP;
            }
    }
    //默认返回
    return SDImageFormatUndefined;
}
```

####15. 后台执行任务

```objective-c
- (void)backgroundTask
{
#if SD_UIKIT
    Class UIApplicationClass = NSClassFromString(@"UIApplication");
    BOOL hasApplication = UIApplicationClass && [UIApplicationClass respondsToSelector:@selector(sharedApplication)];
    if (hasApplication && [self shouldContinueWhenAppEntersBackground]) {
        UIApplication *app = [UIApplicationClass performSelector:@selector(sharedApplication)];
        __weak typeof(self) weakSelf = self;
        self.backgroundTaskId = [app beginBackgroundTaskWithExpirationHandler:^{
            __strong typeof(weakSelf) strongSelf = weakSelf;
            if (strongSelf) {
                [strongSelf cancel];
                [app endBackgroundTask:strongSelf.backgroundTaskId];
                strongSelf.backgroundTaskId = UIBackgroundTaskInvalid;
            }
        }];
    }
#endif
}
```

####16. NSDictionary 不能存 nil 对象

```objective-c

2017-12-18 14:06:34.899980+0800 AIS[271:17097] *** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason:
 '*** -[__NSPlaceholderDictionary initWithObjects:forKeys:count:]: attempt to insert nil object from objects[0]'
NSArray *releaseNotesArray = @[@{@"content":releaseNotes ? @"" : releaseNotes,@"lang":@"en"}];


```
####17. NSMultableArray 不能插入 nil 对象

```objective-c

-[__NSArrayM insertObject:atIndex:]: object cannot be nil
for (NSInteger i = 0; i < 65; i++) {
    NSString *imageName = [NSString stringWithFormat:@"Connect_0000%ld",i];
    UIImage *img = [UIImage imageNamed:imageName];
    [tmpArray addObject:img];
}


```
####18. 修改导航栏返回键

```objective-c


// 细节: 本界面上设置, 下个界面上显示  
// 方式一  
self.navigationItem.backBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"返回" style:UIBarButtonItemStylePlain target:nil action:nil];  
// 方式二  
UIBarButtonItem * backButtonItem = [[UIBarButtonItem alloc] init];  
backButtonItem.title = @"返回";  
self.navigationItem.backBarButtonItem = backButtonItem; 

```
####19. 应用内跳转系统设置对应 URL
```objective-c

Wi-Fi: App-Prefs:root=WIFI
蓝牙: App-Prefs:root=Bluetooth
蜂窝移动网络: App-Prefs:root=MOBILE_DATA_SETTINGS_ID
个人热点: App-Prefs:root=INTERNET_TETHERING
运营商: App-Prefs:root=Carrier
通知: App-Prefs:root=NOTIFICATIONS_ID
通用: App-Prefs:root=General
通用-关于本机: App-Prefs:root=General&path=About
通用-键盘: App-Prefs:root=General&path=Keyboard
通用-辅助功能: App-Prefs:root=General&path=ACCESSIBILITY
通用-语言与地区: App-Prefs:root=General&path=INTERNATIONAL
通用-还原: App-Prefs:root=Reset
墙纸: App-Prefs:root=Wallpaper
Siri: App-Prefs:root=SIRI
隐私: App-Prefs:root=Privacy
定位: App-Prefs:root=LOCATION_SERVICES
Safari: App-Prefs:root=SAFARI
音乐: App-Prefs:root=MUSIC
音乐-均衡器: App-Prefs:root=MUSIC&path=com.apple.Music:EQ
照片与相机: App-Prefs:root=Photos
FaceTime: App-Prefs:root=FACETIME


```