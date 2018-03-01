### iOS 开发中遇到的问题以及解决方案

####1. AFNetworking unacceptable content-type: text/html

***原因***：因为 manager 有一个 responseSerializer 属性，它只设置了一些固定的解析格式，其中不包含text/html这种数据的格式。 
 
***解决方案***： 
 
方案1：在自己的请求中添加该类型  

```Objective-C

 self.acceptableContentTypes = [NSSet setWithObjects:@"application/json", @"text/json", @"text/javascript", nil];
// 添加 text/html 类型到可接收内容类型中
    mgr.responseSerializer.acceptableContentTypes= [NSSetsetWithObjects:@"text/html", nil];

```
好处：如果只是请求一次，那是最佳选择。
弊端：多次请求每次都要添加该代码，麻烦且不方便。
  
方案2：更改框架源码

```Objective-C

 self.acceptableContentTypes = [NSSet setWithObjects:@"text/html",@"application/json", @"text/json" ,@"text/javascript", nil];

```
好处：直接修改框架源代码，每个请求都解析该类型。  
弊端：框架更新后，修改的代码就无效了。

方案3：创建一个类继承自AFHTTPSessionManager，重写父类的manager方法

```Objective-C
+ (instancetype)manager {
    DXHTTPManager *mgr = [super manager];
    // 创建NSMutableSet对象
    NSMutableSet *newSet = [NSMutableSet set];
    // 添加我们需要的类型 
    newSet.set = mgr.responseSerializer.acceptableContentTypes;
    [newSet addObject:@"text/html"];

    // 重写给 acceptableContentTypes赋值
    mgr.responseSerializer.acceptableContentTypes = newSet;

    return mgr;
}
```
好处：用自己创建的类发送请求，易维护易拓展
弊端：继承不太方便。

方案4：Method Swizzle

```Objective-C
#import <AFNetworking/AFNetworking.h>
@interface AFJSONResponseSerializer (Category)
@end

#import" AFJSONResponseSerializer+Category.h"
@implementation AFJSONResponseSerializer (Category)

+ (void)load {
   staticdispatch_once_tonceToken;
   dispatch_once(&onceToken, ^{
   Method orignalMethod =class_getClassMethod([selfclass],@selector(init));            
   Method swizzledMethod   
     =class_getClassMethod([selfclass],@selector(dev4mobile_init));
   method_exchangeImplementations(orignalMethod, swizzledMethod);
  });
}

- (instancetype)dev4mobile_init {
    [self dev4mobile_init];
    self.acceptableContentTypes = [NSSetsetWithObjects:@"application/json",@"text/json",@"text/javascript",@"text/html",nil];
    return self;
}
@end

```
好处：利用 Runtime 交换方法的实现，从而修改 AFJSONResponseSerializer 的 init 方法。

####2. API 设计兼容问题

***原因***：在多个项目复用的底层 SDK API 中，由于 JS 不支持方法的重载导致 Android 接口在添加参数个数的时候无法向下兼容，而 iOS 不存在重载的问题。

***解决方案***：将方法的参数除回调外封装成 JSON 格式对象，不同的项目在调用的时候去解析参数的个数以及类型。

####3. 60x60 的图标压缩成 20x20 失真问题
####4. button 的 highligh 图片延迟显示以及 UIButton 相关问题
####5. 自定义 UITableViewCell 以及相关问题
####6. 添加的手势无效（添加特定手势子类）
####7. 如何获取WIFI加密方式（暂无办法）
####8. 程序崩溃无法启动

>dyld: Library not loaded: @rpath/Neptune.framework/Neptune
  Referenced from: /var/containers/Bundle/Application/E512EA8C-D92E-433A-8826-3A7BC89A4ADE/DongleWIFI助手.app/DongleWIFI助手
  Reason: image not found
  
***原因***：动态库未加载

***解决方案***：
![](/Users/neilzhang/Library/Containers/com.tencent.qq/Data/Library/Application Support/QQ/Users/370682511/QQ/Temp.db/3D97DAA3-D24D-4BC8-8AFE-5047F5ACB95F.png)

####9. 程序崩溃

>*** Terminating app due to uncaught exception 'NSRangeException', reason: '*** -[__NSArray0 objectAtIndex:]: index 0 beyond bounds for empty NSArray'

***原因***：访问一个空数组下标  

***解决方案***：查找代码，进行数组判空

####10. 懒加载无法实时加载数据

####11. 程序崩溃
>EXC_BAD_ACCESS (code=1, address=0x10)

***直接原因***：In short, this type of problem occurs when you release the memory assigned to an object that has been already released.  

***根本原因***：多个线程同时回调，回调函数已经置空，访问到了野指针，不存在的地址。

***解决方案***：加锁

####12. viewcontroller 生命周期
单个控制器：   
init -> viewDidLoad -> viewWillAppear -> viewWillLayoutSubviews -> viewDidLayoutSubviews -> viewDidAppear -> viewWillDisappear -> viewDidDisappear  
多个控制器：当我们点击**push**的时候首先会加载下一个界面然后才会调用界面的消失方法  
1. viewWillDisappear：ViewController1 将要消失  
2. viewWillAppear：ViewController2 将要出现  
3. viewWillLayoutSubviews ViewController2  
4. viewDidLayoutSubviews ViewController2   
5. viewWillLayoutSubviews:ViewController1  
6. viewDidLayoutSubviews:ViewController1  
7. viewDidDisappear:ViewController1 完全消失  
8. viewDidAppear:ViewController2 完全出现  


####13. int a = 2 / 3 (0...)

####14. UINavigationItem 和 UINavigationBar

**UINavigationItem**  

- An object that manages the buttons and views to be displayed in a navigation bar object. 
- When building a navigation interface, each view controller pushed onto the navigation stack must have a UINavigationItem object that contains the buttons and views it wants displayed in the navigation bar. The managing UINavigationController object uses the navigation items of the topmost two view controllers to populate the navigation bar with content.
- When specifying buttons for a navigation item, you must use UIBarButtonItem objects. If you want to display custom views in the navigation bar, you must wrap those views inside a UIBarButtonItem object before adding them to the navigation item.

**UINavigationBar**

![UINavigationBar](/Users/neilzhang/Library/Containers/com.tencent.qq/Data/Library/Application Support/QQ/Users/370682511/QQ/Temp.db/5423E2B4-47CC-4B46-895E-FFBC7DFB76CC.png)

- A control that supports navigation of hierarchical content, most often used in navigation controllers.
- A UINavigationBar object is a bar, typically displayed at the top of the window, containing buttons for navigating within a hierarchy of screens. The primary components are a left (back) button, a center title, and an optional right button. You can use a navigation bar as a standalone object or in conjunction with a navigation controller object.
- To control a navigation bar when using a navigation controller, the following steps are required:

  - Create a navigation controller in Interface Builder or in the code.

  - Configure the appearance of the navigation bar using the navigationBar property on the UINavigationController object.

  - Control the content of the navigation bar by setting the title and navigationItem properties on each UIViewController you push onto the navigation controller’s stack.


####15. 文件转 NSData 失败

**原因：**文件 **URL** 未添加 **file://** 协议头  

**解决方案：** 

>//    self.recorderURL = [NSURL URLWithString:path];
    self.recorderURL = [NSURL fileURLWithPath:path];