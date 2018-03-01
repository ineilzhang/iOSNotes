[TOC]

####1.头文件的引入(@class)

- 将引入头文件的时机尽量延后，只有在确定需要时才引入，这样可以减少类的使用者所需引用的头文件数量，减少编译时间，同时也可以解决头文件的循环引用问题（**chicken-and-egg situation**）。  
- 如非确有必要，否则不要引入头文件。一般来说，应该在某个类的头文件中使用向前声明（**@class**）来声明某个类，并在实现文件中引入该头文件。
- 有时无法使用向前声明（**@class**），如继承某个类或者声明遵守某个协议时，尽量将该声明移至“**class-contination**分类”中。如果不行，就把协议的声明单独放到一个头文件中。


####2.字面量语法（literal syntax）

- 使用字面量语法，少用与之等价的语法可以缩短代码长度，增强代码的可读性。

```objective-c

    //字面量数值
    NSNumber *number = @1;
    NSNumber *number9527 = [NSNumber numberWithInteger:1];
    
    //字面量数组,创建数组时需要注意不能添加nil，否则会报异常
    NSArray *letter = @[@"a",@"b",@"c"];
    //使用与之等价的方法创建时，如果检测到了nil则会提前结束添加元素但是不会报异常
    NSArray *letter9527 = [NSArray arrayWithObjects:@"a",@"b",@"c",nil];
    
    //取数组中的元素
    NSString *a = letter[0];
    NSString *a9527 = [letter9527 objectAtIndex:0];
    
    //字面量字典
    NSDictionary *person = @{@"firstName":@"neil",@"lastName":@"zhang"};
    NSDictionary *person9527 = [NSDictionary dictionaryWithObjectsAndKeys:@"neil",@"firstName",@"zhang",@"lastName",nil];
    
    //取字典数据
    NSString *firstName = person[@"firstName"];
    NSString *firstName9527 = [person objectForKey:@"firstName"];
    
```
- 用字面量语法创建数组或字典时，若元素中有 **nil** 时会报异常，有利于排查错误。

####3.多用类型常量，少用 #define 预处理指令

- 不要用预处理指令定义常量。因为一方面预处理指令只是在预处理阶段执行简单的查找和替换工作，定义出的常量也不包含类型信息，另一方面定义出的常量值也有可能无意中被其他人修改，而编译器并不会发出警告。

```objective-c
#define ANIMATION_DURATION  0.3f
static const NSTimeInterval kAnimationDuration = 0.3f;
```

- 在实现文件中定义的内部常量（**static const**），不在全局符号表中所以无需在其前面添加相关类的前缀，只需添加一个k即可。  

```objective-c
static const NSTimeInterval kAnimationDuration = 0.3f;

```

- 在头文件中声明的全局常量（**extern const**），在相关实现文件中定义其值。这种常量出现在全局符号表中，应该添加与之相关的类名做为前缀。

```objecticve-c 
//.h
extern NSString * const NZConfigViewName;
extern const NSTimeInterval kAnimationDuration;
//.m
NSString * const NZConfigViewName = @"9527";
const NSTimeInterval kAnimationDuration = 0.3f;
```

####4.用枚举类型表示状态,选项,状态码,模式等

```objective-c
typedef NS_ENUM(NSInteger, UIDeviceOrientation) {
    UIDeviceOrientationUnknown,
    UIDeviceOrientationPortrait,            // Device oriented vertically, home button on the bottom
    UIDeviceOrientationPortraitUpsideDown,  // Device oriented vertically, home button on the top
    UIDeviceOrientationLandscapeLeft,       // Device oriented horizontally, home button on the right
    UIDeviceOrientationLandscapeRight,      // Device oriented horizontally, home button on the left
    UIDeviceOrientationFaceUp,              // Device oriented flat, face up
    UIDeviceOrientationFaceDown             // Device oriented flat, face down
} __TVOS_PROHIBITED;

typedef NS_ENUM(NSInteger, UIInterfaceOrientation) {
    UIInterfaceOrientationUnknown            = UIDeviceOrientationUnknown,
    UIInterfaceOrientationPortrait           = UIDeviceOrientationPortrait,
    UIInterfaceOrientationPortraitUpsideDown = UIDeviceOrientationPortraitUpsideDown,
    UIInterfaceOrientationLandscapeLeft      = UIDeviceOrientationLandscapeRight,
    UIInterfaceOrientationLandscapeRight     = UIDeviceOrientationLandscapeLeft
} __TVOS_PROHIBITED;

typedef NS_OPTIONS(NSUInteger, UIInterfaceOrientationMask) {
    UIInterfaceOrientationMaskPortrait = (1 << UIInterfaceOrientationPortrait),
    UIInterfaceOrientationMaskLandscapeLeft = (1 << UIInterfaceOrientationLandscapeLeft),
    UIInterfaceOrientationMaskLandscapeRight = (1 << UIInterfaceOrientationLandscapeRight),
    UIInterfaceOrientationMaskPortraitUpsideDown = (1 << UIInterfaceOrientationPortraitUpsideDown),
    UIInterfaceOrientationMaskLandscape = (UIInterfaceOrientationMaskLandscapeLeft | UIInterfaceOrientationMaskLandscapeRight),
    UIInterfaceOrientationMaskAll = (UIInterfaceOrientationMaskPortrait | UIInterfaceOrientationMaskLandscapeLeft | UIInterfaceOrientationMaskLandscapeRight | UIInterfaceOrientationMaskPortraitUpsideDown),
    UIInterfaceOrientationMaskAllButUpsideDown = (UIInterfaceOrientationMaskPortrait | UIInterfaceOrientationMaskLandscapeLeft | UIInterfaceOrientationMaskLandscapeRight),
} __TVOS_PROHIBITED;

typedef NS_OPTIONS(NSUInteger, UIViewAutoresizing) {
    UIViewAutoresizingNone                 = 0,
    UIViewAutoresizingFlexibleLeftMargin   = 1 << 0,
    UIViewAutoresizingFlexibleWidth        = 1 << 1,
    UIViewAutoresizingFlexibleRightMargin  = 1 << 2,
    UIViewAutoresizingFlexibleTopMargin    = 1 << 3,
    UIViewAutoresizingFlexibleHeight       = 1 << 4,
    UIViewAutoresizingFlexibleBottomMargin = 1 << 5
};
```
- 应该使用枚举类型来表示状态机的状态，传递给方法的选项以及状态码等值，给这些值定义一目了然的名称。
- 如果传递给某个方法的选项可以组合使用，则可以使用 **NS_OPTIONS** 移位的方式表示。
- 在 **switch** 语句中处理枚举类型时不要使用 **default** 语句，在添加新的 **case** 时编译器会显示警告：未处理所有枚举。

####5.属性的概念

- 在对象外访问实例变量时，总是应该通过属性去做。在对象内部访问实例变量是，应该通过实例变量来读，通过属性来写（保证属性的特性实现）。
- 在初始化方法以及 **dealloc** 方法中总是应该通过实例变量来读写数据。
- 使用懒加载时，应该通过属性来读取数据。

####6.isEqual

- **NSSet : A static unordered collection of unique objects.**

- **collection** 会把各个对象按照其哈希值放到不同的“箱子数组”中，所以某个对象放到 collection 中之后就不应该在改变其哈希值。
- 自定义两个对象相等的方法，需要重写 **isEqual：** 和 **hash** 方法。
- 相同的两个对象具有相同的 **hash** 值，但具有相同的哈希值的两个对象并不一定相等。
- **hash** 方法的设计应该权衡计算速度和碰撞几率。

####7.使用 Class Clusters（类簇）隐藏实现细节

```objective-c
//NZEmployee.h
#import <Foundation/Foundation.h>

typedef NS_ENUM(NSInteger,NZEmployeeType){
    NZEmployeeTypeEnginer = 0,
    NZEmployeeTypeDesigner,
    NZEmployeeTypeFanince,
};

@interface NZEmployee : NSObject

@property (nonatomic,copy) NSString *name;
@property (nonatomic,assign) NSInteger salary;

+ (instancetype)employeyeeWithType:(NZEmployeeType)type;
- (void)doADaysWork;

@end


//NZEmloyee.m
#import "NZEmployee.h"
#import "NZEmployeeEnginer.h"
#import "NZEmployeeDesigner.h"
#import "NZEmployeeFanince.h"

@implementation NZEmployee

+ (instancetype)employeyeeWithType:(NZEmployeeType)type
{
    switch (type) {
        case NZEmployeeTypeEnginer:
            return [NZEmployeeEnginer new];
            break;
        case NZEmployeeTypeFanince:
            return [NZEmployeeFanince new];
            break;
        case NZEmployeeTypeDesigner:
            return [NZEmployeeDesigner new];
            break;
        default:
            break;
    }
}

- (void)doADaysWork
{
    //subClasses implement this.
}

@end
```
- 类簇模式可以把实现细节隐藏在简单的公共接口后面。
- 系统框架中经常使用类簇。
- **Objective-C** 语言无法指明某个基类是抽象的，所以采用类簇模式实现的基类一般没有 **init** 方法，暗示使用者不应该直接创建该类的实例。

####8.在既有类中使用关联对象存放自定义数据

- 可以通过关联对象机制将两个对象l连接起来。
- 定义关联对象时可以指定内存管理语义，用来模仿属性。
- 只有在其他方法无法使用的情况下才使用关联对象，因为可能会产生难以排查的 **bug**。

####9.消息发送机制（objc_msgSend）

- 消息由接受者，选择子和参数构成。给某个对象发送消息即是相当于在该对象上调用方法。
- 发送给某个对象的全部消息都由动态消息派发系统来处理，该系统会查找出相应的方法并执行。

####10.消息转发机制

```objective-c
//动态方法解析
+ (BOOL)resolveInstanceMethod:(SEL)sel
{
    NSString *selString = NSStringFromSelector(sel);
    if ([selString hasPrefix:@"set"]) {
        class_addMethod(self, sel, (IMP)autoDictionarySetter, "v@:@");
    }else{
        class_addMethod(self, sel, (IMP)autoDictionaryGetter, "@@:@");
    }
    return NO;
}
void autoDictionarySetter(id self,SEL _cmd,id value)
{
    NZAutoDictionary *typedSelf = (NZAutoDictionary *)self;
    NSMutableDictionary *storeDic = typedSelf.storeDic;
    NSString *setterString = NSStringFromSelector(_cmd);
    NSMutableString *key = [setterString mutableCopy];
    [key deleteCharactersInRange:NSMakeRange(key.length - 1, 1)];
    [key deleteCharactersInRange:NSMakeRange(0, 3)];
    NSString *firstLowcaseString = [[key substringToIndex:1] lowercaseString];
    [key replaceCharactersInRange:NSMakeRange(0, 1) withString:firstLowcaseString];
    if (value) {
        [storeDic setObject:value forKey:key];
    }else{
        [storeDic removeObjectForKey:key];
    }
}
id autoDictionaryGetter(id self,SEL _cmd)
{
    NZAutoDictionary *typedSelf = (NZAutoDictionary *)self;
    NSMutableDictionary *storeDic = typedSelf.storeDic;
    NSString *key = NSStringFromSelector(_cmd);
    return [storeDic valueForKey:key];
}

//转发给其他接收者处理未知的选择子aSelector
- (id)forwardingTargetForSelector:(SEL)aSelector
{
    return self.object;//self.object能够处理aSelector
    return nil;//没有备用对象能够处理aSelector
}

//完整的消息转发机制
- (NSMethodSignature *)methodSignatureForSelector:(SEL)aSelector
{
    NSMethodSignature *signature = [super methodSignatureForSelector:aSelector];
    if (!signature) {
        signature = [self.object methodSignatureForSelector:aSelector];
    }
    return signature;
}

- (void)forwardInvocation:(NSInvocation *)anInvocation
{
    SEL sel = anInvocation.selector;
    if ([self.object respondsToSelector:sel]) {
        [anInvocation invokeWithTarget:self.object];
    }else{
        [self doesNotRecognizeSelector:sel];
    }
}
```

####11.method swizzling(方法调配)

```objective-c

#import "NSString+NZLog.h"

@implementation NSString (NZLog)

- (NSString *)myLowercaseString
{
    NSString *lowerCase = [self myLowercaseString];
    NSLog(@"lowerCase = %@",lowerCase);
    return lowerCase;
}

@end

//  method swizzling
    Method originLowercaseMethod = class_getInstanceMethod([NSString class], @selector(lowercaseString));
    Method myLowercaseMethod = class_getInstanceMethod([NSString class], @selector(myLowercaseString));
    method_exchangeImplementations(originLowercaseMethod, myLowercaseMethod);

```

####12.使用前缀避免命名空间冲突

- 使用公司或者应用相关的名称作为类名的前缀。
- 如果程序中使用到了第三方库，则应该为其名称加上前缀。

####13.指定初始化方法（designated initiation）

- 在类中提供一个或多个指定初始化方法，其他初始化方法均应调用此初始化方法。
- 如果指定初始化方法与父类不同，则应该覆写父类中的方法。
- 如果父类中的指定初始化方法不适用与子类，则应该覆写父类方法并抛出异常。

####14.尽量使用不可变对象

- 尽量创建不可变对象。
- 若某属性仅可于对象内部修改，则在 **class-continuation** 分类中将其扩展由只读扩展为可读写。
- 不要把可变的集合作为属性公开，应该提供相关的方法修改其中的对象。

####15.Objective-C 错误模型

- 只有发生了可能导致整个应用程序崩溃的严重错误才应该使用异常。
- 在错误不那么严重的情况下，可以使用 **NSError** 对象。第一种常用的方式是使用 **NSError** 对象的一级指针通过委托对象来传递错误，第二种是使用 **NSError** 二级指针将错误信息放到 **NSError** 对象中经过输出参数返回给回调者。

####16.NSCopying 协议

- 如果想要使自己写的对象具有拷贝功能，需要实现 **NSCopying** 协议。如果有不可变的版本，还要实现 **NSMutableCopying** 协议。
- 复制对象时，需要考虑是执行深拷贝还是浅拷贝。一般情况下尽量执行浅拷贝，如需执行深拷贝需要新增一个专门执行深拷贝的方法。

####17.通过委托和数据源协议进行对象间通信

- 委托模式为对象提供了一套接口，使其可以由此接口将x相关事件通知其他对象。
- 如果有必要可以使用位段的结构体将委托对象是否能响应相关协议方法缓存其中。

####18.将类的实现代码分散到数个便于管理的分类中

- 使用分类机制将类的实现代码划分成易于管理的小模块。
- 将应该视为私有的方法归入名为 **Private** 的分类中，隐藏其实现。
- 在向第三方类添加分类时，为了防止方法重名，总是应该给分类名称和方法名称添加前缀。

####19.不要在分类中声明属性

>类所封装的数据都应该定义在主接口中，这是唯一能够定义实例变量的地方。至于分类机制，目标在于扩展类的功能，而非封装数据。

####20.

