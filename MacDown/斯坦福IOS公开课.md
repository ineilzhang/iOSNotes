PK
    $�iL���2	  2	   ! 斯坦福IOS公开课.mdup �S�斯坦福IOS公开课.md[TOC]
### 斯坦福IOS公开课
#### 1. 第一节课
##### 1. What's in IOS ?

- Cocoa Touch
	1. Mutil-Touch -->Alerts
	2. Core Motion -->Web View
	3. View Hierarchy -->Map Kit
	4. Locallization -->Image Picker
	5. Controls -->Camera
- Media
	1. Core Audio -->JPEG,PNG,TIFF
	2. OpenAL -->PDF
	3. Audio Mixing -->Quartz(2D)
	4. Audio Recoding -->Core Animation
	5. Video Playback -->OpenGL ES
	
- Core Services
	1. Collectins -->Core Location
	2. Address Book -->Net Service
	3. Networking -->Threading
	4. File Access -->Preferences
	5. SQLite -->URL Utilities
- Core OS
    1. OSX Kernel -->Power Managerment
    2. Mach 3.0 -->Keychain Access
    3. BSD -->Certficates
    4. Sockets -->File System
    5. Security -->Bonjour
##### 2. Platform Components 
- Tools 
	1. Xcode
	2. Instruments
- Language
  - [display setTextColor:[UIColor blackColor]];
- Frameworks
	1. Foundation
	2. Core Data
	3. Map Kit
	4. UIKit 
	5. Core Motion
- Design Strategies
		- MVC
##### 3. MVC

1. Divide objects in your program into 3 "camps".
2. what mean is MVC?
![Alt text](./MVC模型.jpg)

  - Model = What your application is. 
  - View = Your Controller's minions.
  - Controller = How your Model is presented to the user.
##### 4. Objective-C 
- strong and weak, nonatomic ,synthesize
  - **strong** means:
> "keep the object that this proprety points to in memory until I set this property to **nil**(zero)(and it will stay in memory until everyone who has a **strong** pointer to it sets their property to **nil** too)"

  - **weak** means:
> "if no one else has a **strong** pointer to this object, then you can throw it out of memory and set this property to **nil**(this can happen at any time)"

  - **nonatomic** means:
> "access to this property is not thread-safe". We will always specify this for objects in this course.If you do not,then the compiler will generate locking code that will complicate your code elsewhere.

  - **synthesize** means:
> "This @synthesize is the line of code that actually creates the baking instance variable is set and gotten."
- setter and getter 
```
@(学习笔记本)synthesize contents = _contents;
//It is actually possible to change the name of the getter that is generated.
//- (NSString *)isContents
- (NSString *)contents
{
	return _contens;
}

- (void)setContents:(NSString *)contents
{
	_contents = contents;
}
PK 
    $�iL���2	  2	   !               斯坦福IOS公开课.mdup �S�斯坦福IOS公开课.mdPK      g   �	    