[TOC]
### Lecture 3 Slides

#### 1.  Classes
Header.h(public) versus Implementation.m(private)
**@interface** MyClass : MySuperclass ... **@end**(only in header file)
**@interface** MyClass() ... **@end**(only in implementation file)
**@interface** ... **@end**(only in implementation file)
*#import*
#### 2. Properties
**@property (nonatomic)** [type]  [property name] 
It's just setter and getter methods. Defaulet ones automatically generated for you by compiler.Better than instance variables alone(lazy instantiation, consistency checking, UI updateing, etc.).
**@property (strong ro weak)** [type which is a pointer to an object]  [property name ]
**@property (getter=《getter name》)** ...
**@property (readonly)** ... & **@property (readwrite)** ...
Invoking setter and getter using _dot natation_,e.g. , self.cards = ... or if (rank > self.rank) ...
**@synthesize** [prop name] = _[prop name] (only if you implement  **both**  setter **and** getter )
#### 3. Types and Memory
Types: MyClass * , BOOL (YES or NO), int, NSUInteger, etc.
All objects live in the heap(i.e. we only have **pointer** to them).
Object storage in the heap is managed automatically (guiaded by **strong** and **weak** declarations).
Lazy instantiation (using a **@property**'s getter to **alloc**ate and **init**ialize the object that the **@property** points to in an "on demand " fasion ). Not everything is lazily instantiated, btw. :)
If a pointer has the value nil (i.e. 0), it means the pointer does not point to anything.
#### 4. Methods 
Declaring and defining instance methods, e.g., **- (int)match:(NSArray *)otherCards**
Declaring and defining class methods, e.g., **+ (NSArray *)validSuits**
Invoking instance methodss, e.g., **[myArray addobject:anobject]**
Invoking class methods, e.g.,  unsigned int rank = **[PlayingCard maxRank]**
Method's name and its parameters are interspersed, e.g., **[deck addCard:aCard atTop:YES]**
#### 4. NSString
_Immutable_ and usually created by manipulating other strings or **@""** notaion or class methods.
e.g. NSString *myString = @"hello"
e.g. NSString *myString = [otherString stringByAppendString:yetAnotherString]
e.g. NSString *myString = [NSString stringWithFormat:@"%d%@", myInt, myObj]
#### 5. Creating Objects in the Heap
Allocation (NSObject's alloc) and initialization (with and init... method) _always_ happen together!
e.g. [[NSMutableArryay alloc] init]
e.g. [[CardMatchingGame alloc] initWithCardCount:c usingDecd:d]
