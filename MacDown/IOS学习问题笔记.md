[TOC]
### IOS学习问题笔记

@(学习笔记本)

#### 2016.9.18 第五章CollectionView
1. 问题描述：集合视图无法绘制出来，main函数执行到，viewDidLoad没有执行到。
原因：
解决办法：
#### 2016.9.18 14:15 第六章SimpleTableView
1. 问题描述：无法填充数据到单元格中，numberOfSectionsInTableView:函数执行到，tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath函数没有执行，
原因：方法搞错了，必须要是实现的是numberOfRowsInSection:方法。
解决办法：实现numberOfRowsInSection:方法。
总结：编码粗心，不仔细。
2. 问题描述：单元格中没有显示图片
原因：imagePath = [imagePath stringByAppendingString:@".png"];忘记“. png”
解决办法：加上" . "
总结：编码粗心 + 1，不仔细 + 1  
#### 2016.9.18 15:45 第六章CustomeCell
1. 问题描述：无法非配可重用的Cell
原因：没有指定cell的ID，无法分配地址给cell
解决办法：选定customcell的属性检查器填写identifier(该方法是使用代码和Interface Builder相结合的方式来获得可从用的单元格，还有纯代码的方式)
总结：没有理解tableview的构建过程，盲目的copy代码，没有逻辑思路
#### 2016.9.18 17:43 第六章SearchBar
1. 问题描述：搜索框输入就崩溃，搜索到了数据，但是无法reloadData.
原因：
解决办法：
总结：
#### 2016.9.19 14:17 第七章ModalView
1. 问题描述：[self dismissViewControllerAnimated:YES completion:^{}];
completion的参数？？？
答案：闭包，相当于一个参数
2. 问题描述：selector:@selector(registerCompletion:)？selector ？
答案：Google
3. 问题描述：点击注册按钮后，没有进入到登录界面控制器，viewDidLoad函数没有运行到。
原因：没有指定登录视图控制器的custom class。
解决办法：选择登录视图控制器 ，打开其标示检查器，选择class为viewcontroller
总结：步骤增多之后，没有清楚的逻辑，一步步该如何做。
#### 2016.9.20 9:54 第七章TreeNavigation
1. 问题描述：进入到一级界面之后无法跳转到二级界面，跳转指标显示为灰色。
原因：segue连线出错，应该是从cell处连接到二级界面
解决办法：删除原有的segue，重新从cell连接。
总结：看书不仔细 + 1。
#### 2016.9.21 17:06 第15章MKNetworkKit
1. 问题描述：下载程序进行时，首先运行一下
[downloadOperation addCompletionHandler:^(MKNetworkOperation *operation) {}
之后完成请求之后再次调用。
原因：闭包，相当于参数的检查。
2. if(NSData data)怎么运行？
总结：点进去👀
3. NSArray *paths = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, 
NSUserDomainMask, YES);？？？什么意思？
总结：C语言C语言C语言，点进去👀
#### 2016.9.28 21:17 MyMeiTuan
1. 问题描述：修改button的title的时候，字体的颜色不改变，代码如下
``` objectivec
button.titleLabel.textColor = [UIColor redColor];
```
原因：点击titleLabel源码，发现如下：
> Do not use the label object to set the text color or the shadow color. Instead, use the setTitleColor:forState: and setTitleShadowColor:forState: methods of this class to make those changes.

解决办法：
``` objectivec
[button setTitleColor:[UIColor redColor] forState:UIControlStateNormal];
```
总结：多多点击源码进去看，大有裨益。

#### 2016.11.11 11:30:20 youlook

1. 问题描述：自定义单元格内容绘制时没有生效，一片空白，代码如下：
``` objectivec
TSWifiLevelView *cell = [tableView dequeueReusableCellWithIdentifier:@"cel222" ];
if (cell == nil) {
         cell = [[UITableViewCell alloc]initWithStyle:UITableViewCellStyleValue1 reuseIdentifier:kTSRouterManagerCellidentifier];
      [cell setSelectionStyle:UITableViewCellSelectionStyleNone];
    }
//其中 TSWifiLevelView 是自定义的 cell
```
原因： 自定义的 cell 不能使用系统定义的 style 。
解决办法：
```objectivec
if (cell == nil) {
      cell = [[TSWifiLevelView alloc]init];
      [cell setSelectionStyle:UITableViewCellSelectionStyleNone];
    }
```
总结：搞清楚自定义的意义。

2. 