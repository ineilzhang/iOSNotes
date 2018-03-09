### iOS framework 制作

#### 前言

如果你想将你开发的控件与别人分享，一种方法是直接提供源代码文件。然而，这种方法并不是很优雅。它会暴露所有的实现细节，而这些实现你可能并不想开源出来。此外，开发者也可能并不想看到你的所有代码，因为他们可能仅仅希望将你的这份代码的一部分植入自己的应用中。

另一种方法是将你的代码编译成静态库（library），让其他开发者添加到自己的项目中。然而，这需要你一并公布所有的公开的头文件，实在是非常不方便。

你需要一种简单的方法来编译你的代码，这种方法应该使得你的代码易分享，并且在多个工程中易复用。你需要的是一种方法来打包你的静态库，将所有的头文件放到一个单元中，这样你就可以立刻将其加入到你的项目中并使用。

#### 步骤

1. 打开 Xcode ，创建 Cocoa Touch FrameWork 工程![Alt text](./1502776254110.png)

2. 创建并实现功能类![Alt text](./1502776356615.png)

3. 将需要公开的头文件集中到 FrameWork 同名的自动生成的头文件中，并设置相关的 Build Phase![Alt text](./1502776470125.png)![Alt text](./1502776578342.png)

4. 修改 Build Setting 相关设置![Alt text](./1502777395628.png)![Alt text](./1502776898519.png)![Alt text](./1502777057327.png)  


5. commend + B
如果需要支持模拟器和手机，则需要在连接手机时和模拟器下分别编译。

#### 打包 FrameWork

打包 FrameWork有两种方式：
1. 手动编译打包合并  
    - 测试编译出的库所支持的架构
      - 右键点击 Product 目录下的 XXX.framework Show in Finder    
      - 进入到 XXX.framework 下，执行 ```lipo -info```命令,可以看出其支持的 CPU 架构![Alt text](./1502778018222.png)
    - 如果需要该 FrameWork 同时支持模拟器和手机，则需要将编译出的两个 FrameWork 合并，可以使用 ```lipo -create xxx xxx -output xxx```命令![Alt text](./1502778673640.png)  
  - 将合并后的库文件替换掉原来只支持单一架构的库文件
  - 大功告成  
  
2. 创建新的 Target 打包
  - 创建新的 Target![Alt text](./1502849815337.png)
  - 选择新建的 Target ，添加脚本![Alt text](./1502849925335.png)
  - 添加脚本![Alt text](./1502850208404.png)
  - command + B，保存生成的 FrameWork

#### FrameWork 的使用

1. 直接将 FrameWork 添加到工程中

2. 添加 Target Build Phase 选项![Alt text](./1502779656002.png)![Alt text](./1502779859789.png)

3. 导入公开的头文件![Alt text](./1502850417712.png)



