[toc]
###Slides Four
####Creating Objects
- Most of the time, we create objects with alloc and init...
**NSMutableArray *cards = [[NSMutableArray alloc]init];**

- Or with class methods 
**NSString *moltuae = [NSString stringWithFormat:@"%d", 42];**

- Sometimes both a class creator method and init method exist
**[NSString stringWithFormat:...] same as [[NSString alloc] initWithFormat:...]**     

-  You can alse ask other objects to create new objects for you
**(NSStirng *)stringByAppendingString:(NSString *)otherString;**

-  But not all objects given out by othrer objects ara newly created
**(id)lastObject;**

####nil
- Sending message to **nil** is (mostly) okay. No code exectued.
If the method returns a value, it will return zero.
**int i = [obj methodWhickReturnAnInt]**;//i will be zero if **obj** is **nil**.
But if the method returns a _C stuct_.Return value is undefined.
**CGPoint p = [obj getLocation];**//**p** will have an undifined value if **obj** is **nil**.

####Dynamic Binding

- Objective-C has an important type called **_id_**
It means "pointer to an object of unknown/unspecified" type.
**id myObject;**
Really _all_ object pointers are treated like **id** _at runtime_.
Figurin out the code to execute when a message is sent _at runtime_ is called "**dynamic binding**".
- Is it safe?
We mostly use static typing (e.g. NSString *) and the compiler is really smart.
- Static typing
**NSString *s = @"x"**; // "statically" typed(compiler will warn if **s** is sent non-NSString messages).
**id obj = s;** //_not_ statically typed, but perfectlly legal; compiler can't catch [obj rank]
**NSArray *a = obj**; // also legal, but obviously could lead to some big troubel.

####Object Typing

- 子类可以访问父类方法。
- 子类可以向上转型为父类。
- 父类不能访问子类方法。
- id 类型可以**_response to _**任何已知方法。
- 静态类型可以**_response to_**任何本类及父类方法。
- 强制类型转化编译时不会报错。

####Dynamic Binding

- Introspection
Asking at runtime what class an object is or what message can be sent to it.
- Protocols
A syntax that is "in between" **id** and static typing.
Does not specify the class of an object pointed to, but does specify what methods it implements.

####Introspection

- All objects that inherit from NSObject know these methods...
**isKindOfClass:**
**isMemberOfClass:**
**responsesToSelector:**
- Arguments to these methods are a little tricky
- **SEL** is the Objective-C "type" for a selector.
**SEL shootSelector = @selector(shoot)**
- If you have a **SEL**, you can also ask an object to perform it...
**[obj performSelector:shootSelector]** 

####NSMutableArray
- Looping through members of an array in an efficient manner(for-in).

####NSNumber

_Object wrapper_ around primitive types like int, float, double, BOOL, enums, etc.
**NSNumber *n = [NSNumber numberWithInt:36];**
**float f = [n floatValue];** // would return 36.0 as a float.

New syntax for creating an **NSNumber** in iOS 6: @()
**NSNumber *three = @(3);**

####NSValue 
Generic object warpper for some non-object, non-primitive data types(i.e. C structs).
e.g. **NSValue *edgeInsetsObject = [NSValue valueWithUIEdgeInsets:UIEdgeInsetsMake(1,1,1,1)];**

####NSData
"Bag of bits." Used to save/restore/transmit raw data throughout the iOS SDK.

####NSDate
Used to find the time right now or to store past or future times/dates.

####NSSet/NSMutableSet
Like an array, but no ordering (no objectAtIndex: method).

####NSOrderedSet / NSMutableOrderedSet
Sort of a cross between NSArray and NSSet.
Objects in an ordered set are **_distinct_**. You can't put the same object in multiple times like array.

####NSDictionary
Immutable collection of objects looked up by a key (simple hash table).
All keys and values are held onto **strong**ly by an NSDictionary.

####NSMutableDicitionary
- Mutable version of NSDictionary.
- Looping through the keys or values of a dicitionary(for-in).

####Property List
- The term "Property List" just means a collection of collections.
It's just a phrase (not a language thing). It means any graph of objects containing only: **NSArray, NSDictionary, NSNumber, NSString, NSData, NSDate(or mutable subclasses thereof)**

####Other Foundation

- NSUserDefaults
Lightweight storage of Property Lists.
It's basically an **_NSDicitionary_ ** that persists between launches of your applicaton.
Not a full-on database, so only store small things like user preferences.
**[[NSUserDefaults standardUserDefaults] setArray:rvArray forKey:@"xxx"];**
Always remember to write the defaults out after each batch of chages!
**[[NSUserDefaults standardUserDefaults] synchronize];**

- NSRange
C struct (not a class)
Used to specify subranges inside strings and arrays(et. al.).
>typedef struct {
	NSUInterger location;
	NSUInteger length;
} NSRange;  

Important **locatin** value **NSNotFound**.  
>NSString *greeting = @"hello world";
NSString *hi = @"hi";
NSRange r = [greeting rangeOfString:hi];
if (r.location == NSNotFound){/* couldn't find hi inside greeting */}

- Colors
  - **UIColor** An object representing a color.
- Fonts 
