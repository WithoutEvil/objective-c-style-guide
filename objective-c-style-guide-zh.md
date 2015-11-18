#Objective-C 代码规范

## 介绍

我们制定此代码规范的原因是，我们可以在我们的项目中保持代码优雅和一致，即使我们会有不同的作者来参与项目。

## 背景

这里有些苹果官方的代码规范文档。如果这里有些东西没有提及，可以在以下文档中了解更多细节:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## 目录

* [语言](#language)
* [代码组织](#code-organization)
* [留白](#spacing)
* [注释](#comments)
* [命名](#naming)
  * [下划线](#underscores)
* [方法](#methods)
* [变量](#variables)
* [属性特性](#property-attributes)
* [点符号语法](#dot-notation-syntax)
* [字面值](#literals)
* [常量](#constants)
* [枚举类型](#enumerated-types)
* [Case 语句](#case-statements)
* [私有属性](#private-properties)
* [布尔值](#booleans)
* [条件语句](#conditionals)
  * [三目操作符](#ternary-operator)
* [Init方法](#init-methods)
* [类构造器方法](#class-constructor-methods)
* [CGRect函数](#cgrect-functions)
* [黄金路径](#golden-path)
* [错误处理](#error-handling)
* [单例模式](#singletons)
* [Xcode 工程](#xcode-project)


<a name="language"></a>
## 语言

应该使用US英语。

**推荐:**
```objc
UIColor *myColor = [UIColor whiteColor];
```

**不推荐:**
```objc
UIColor *myColour = [UIColor whiteColor];
```

<a name="code-organization"></a>
## 代码组织
在函数分组和protocol/delegate实现文件中，使用`#pragma mark -`来分类方法，要遵循以下一般结构:

```objc
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Protocol conformance
#pragma mark - UITableViewDataSource
#pragma mark - CustomDelegate

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

<a name="spacing"></a>
## 留白

* 缩进使用`tab`键
* 方法花括号和其它花括号(`if`/`else`/`switch`/`while` 等.)总是在同一行语句中打开，但在新行中关闭。

**推荐:**
```objc
if (user.isHappy) {
  //Do something
} else {
  //Do something else
}
```

**不推荐:**
```objc
if (user.isHappy)
{
    //Do something
}
else {
    //Do something else
}
```

* 在方法之间应该有且只有一行，这样有利于在视觉上清晰和有组织感。在方法内部的空行应该用于分离功能，但通常应该抽离出来一个新方法。
* 优先使用auto-synthesis。但如果有必要，`@synthesize` 和 `@dynamic`应该在实现文件中被声明，每个声明占一行。
* 应该避免以冒号对齐的方式调用方法。因为有时一个方法签名可能会有3个以上的冒号，冒号对齐会使代码更加易读。请**不要**这样做，尽管冒号对齐的方法包含代码块，因为Xcode的对齐方式令它难以辨认。

**推荐:**

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**不推荐:**

```objc
// colon-aligning makes the block indentation hard to read
[UIView animateWithDuration:1.0
                 animations:^{
                     // something
                 }
                 completion:^(BOOL finished) {
                     // something
                 }];
```

<a name="comments"></a>
## 注释

当需要注释时，注释应该被用来解释一段特殊代码为什么要那样做。任何被使用的注释都应该保持最新或被删除。

一般应该避免使用块注释，因为代码应该尽可能做到自解释，只有当断断续续、几行代码时才需要注释。**例外：这不适用于生成文档的注释**。

<a name="naming"></a>
## 命名

Apple命名规范应该尽可能坚持，特别是那些与[内存管理规则](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508))相关的。

长的、描述性的方法和变量命名是好的。

**推荐:**

```objc
UIButton *settingsButton;
```

**不推荐:**

```objc
UIButton *setBut;
```

三个字符前缀应该总是用在类和常量命名，尽管在Core Data的实体名中可以被忽略。

常量应该使用驼峰命名法:所有的单词首字母应该大写，以相关类名作为前缀，这样清晰明了。

**推荐:**

```objc
static NSTimeInterval const MGSTutorialViewControllerNavigationFadeAnimationDuration = 0.3;
```

**不推荐:**

```objc
static NSTimeInterval const fadetime = 1.7;
```

属性也应该使用驼峰命名法，但首单词的首字母应该小写。对属性使用auto-synthesis，而不是手动编写@synthesize，除非你有一个好的理由。

**推荐:**

```objc
@property (strong, nonatomic) NSString *descriptiveVariableName;
```

**不推荐:**

```objc
id varnm;
```

<a name="underscores"></a>
### 下划线

当使用属性时，实例变量应该总是使用`self.`来访问和改变。这就意味着所有属性视觉上清晰,因为它们前面都有`self.`。

但有一个特例:在初始化方法(initializers)中，实例变量(例如,_variableName)应该直接被使用来避免getters/setters潜在的副作用。

局部变量不应该包含下划线。

<a name="methods"></a>
## 方法

在方法签名中，应该在方法类型(-/+ 符号)之后有一个空格。在方法片段之间应该有一个空行(符合Apple的风格)。总是在参数之前包含一个具有描述性的关键词来描述参数。

“and”这个词的用法应该保留。它不应该用于关联多个参数，就像`initWithWidth:height:`这个例子:

**推荐:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**不推荐:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```


<a name="variables"></a>
## 变量

变量尽量以描述性的方式来命名。单个字符的变量命名应该尽量避免，除了在`for()`循环。

星号表示变量是指针。例如: `NSString *text`， 既不是 `NSString* text` 也不是 `NSString * text`,除了[常量](#constants)的情形。

[私有属性](#private-properties)应该尽可能代替实例变量使用。尽管使用实例变量是一种有效的方式，但更偏向于使用属性来保持代码的一致性。

直接访问实例变量(通过'back'属性:_variable, 变量名前面有下划线)应该避免，除了在初始化方法(`init`, `initWithCoder:`, 其它…),`dealloc` 方法和自定义的 setters 和 getters。更多关于在Initializer和dealloc中使用存取器方法的信息看[这里](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)。

**推荐:**

```objc
@interface MGSTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**不推荐:**

```objc
@interface MGSTutorial : NSObject {
  NSString *tutorialName;
}
```

<a name="property-attributes"></a>
## 属性特性

属性特性应该显示地罗列出来，有助于新的开发者阅读代码。属性的顺序应该是storage、 atomicity，与在Interface Builder连接UI元素时自动生成代码一致。

**推荐:**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```

**不推荐:**

```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

可以改变值的属性(例如:NSString)应该使用`copy`，而不是`strong`。
为什么？即使你声明一个`NSString`的属性，有人可能传入一个`NSMutableString`的实例，然后在你没有注意的情况下修改它。

**推荐:**

```objc
@property (copy, nonatomic) NSString *tutorialName;
```

**不推荐:**

```objc
@property (strong, nonatomic) NSString *tutorialName;
```

<a name="dot-notation-syntax"></a>
## 点符号语法

点语法是对存取器方法调用的一种纯粹地便利封装。当你使用点语法时，属性仍然通过getter和setter方法来访问和改变。阅读更多[here](https://developer.apple.com/library/ios/documentation/cocoa/conceptual/ProgrammingWithObjectiveC/EncapsulatingData/EncapsulatingData.html)

应该总是使用点符号来访问和修改属性，因为它使代码更加简明。方括号更偏向于用在其他实例。

**推荐:**
```objc
NSInteger arrayCount = [self.array count];
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**不推荐:**
```objc
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

<a name="literals"></a>
## 字面值

`NSString`, `NSDictionary`, `NSArray`, 和 `NSNumber`的字面值应该在创建这些类的不可变实例时被使用。请特别注意`nil`值不能传入`NSArray` 和 `NSDictionary`字面值，因为这样会导致crash。

**推荐:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**不推荐:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

<a name="constants"></a>
## 常量

常量是容易重复被使用和无需通过查找和代替就能快速修改值。常量应该使用`static`来声明而不是`#define`，除非显式地使用宏。

**推荐:**

```objc
static NSString * const MGSAboutViewControllerCompanyName = @"meigesir.com";

static CGFloat const MGSImageThumbnailHeight = 50.0;
```

**不推荐:**

```objc
#define CompanyName @"meigesir.com"

#define thumbnailHeight 2
```


<a name="enumerated-types"></a>
## 枚举类型

当使用`enum`时，推荐使用新的固定基本类型，因为它有更强的类型检查和代码补全。现在SDK包含一个宏`NS_ENUM`来帮助和鼓励使用固定的基本类型。

**例如:**

```objc
typedef NS_ENUM(NSInteger, MGSLeftMenuTopItemType) {
  MGSLeftMenuTopItemMain,
  MGSLeftMenuTopItemShows,
  MGSLeftMenuTopItemSchedule
};
```

你也可以显示地赋值(展示旧的k-style常量定义):

```objc
typedef NS_ENUM(NSInteger, MGSGlobalConstants) {
  MGSPinSizeMin = 1,
  MGSPinSizeMax = 5,
  MGSPinCountMin = 100,
  MGSPinCountMax = 500,
};
```

旧的k-style常量定义应该被**避免**，除非编写CoreFoundation C的代码(不太可能).

**不推荐:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```


<a name="case-statements"></a>
## Case 语句

花括号在case语句中并不是必须的，除非编译器强制要求。当一个case语句包含多行代码时，花括号应该加上。

```objc
switch (condition) {
  case 1:
    // ...
    break;
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  case 3:
    // ...
    break;
  default: 
    // ...
    break;
}

```

当相同代码被多个case语句使用时，一个fall-through应该被使用。一个fall-through就是移除case语句中的'break'，这样允许执行流跳转到下一个case。为了代码清晰，一个fall-through应该注释一下。

```objc
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default: 
    // ...
    break;
}

```

当在switch使用枚举类型时，'default'是不需要的。例如：

```objc
MGSLeftMenuTopItemType menuType = MGSLeftMenuTopItemMain;

switch (menuType) {
  case MGSLeftMenuTopItemMain:
    // ...
    break;
  case MGSLeftMenuTopItemShows:
    // ...
    break;
  case MGSLeftMenuTopItemSchedule:
    // ...
    break;
}
```


<a name="private-properties"></a>
## 私有属性
私有属性应该在类的实现文件中的类扩展(匿名类别)中声明。命名类别(比如`MGSPrivate`或`private`)应该从不使用除非是扩展其他类。匿名类别可以使用<headerfile>+Private.h文件命名规则共享/暴露给测试。

**例如：**

```objc
@interface MGSWebViewController ()

@property (strong, nonatomic) UIWebView *webView;
@property (strong, nonatomic) UIToolbar *toolbar;

@end
```

<a name="booleans"></a>
## 布尔值
Objective-C 使用 `YES` 和 `NO`。为此 `true` 和 `false` 应该只在CoreFoundation，C或C++代码中使用。由于 `nil` 解析成 `NO`，所以没有必要在条件语句中比较它。不要拿某样东西直接与 `YES` 比较，因为 `YES` 被定义为1和一个 `BOOL` 能被设置为 `8` 位。

这样确保在不同文件之中保持一致性和在视觉上更加简洁。

**推荐:**

```objc
if (someObject) {}
if (![anotherObject boolValue]) {}
```

**不推荐:**

```objc
if (someObject == nil) {}
if ([anotherObject boolValue] == NO) {}
if (isAwesome == YES) {} // Never do this.
if (isAwesome == true) {} // Never do this.
```

如果 `BOOL` 属性的名字是一个形容词，属性就能忽略"is"前缀，但要指定get访问器的惯用名称。例如：

```objc
@property (assign, getter=isEditable) BOOL editable;
```  
文字和例子引自: [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

<a name="conditionals"></a>
## 条件语句

条件语句主体应该总是使用大括号来防止错误，甚至可以不用大括号的情况(例如：只有一行代码)。这些错误包括，添加第二行代码，期望会成为if条件语句的一部分。此外，[甚至更危险的缺陷]((http://programmers.stackexchange.com/a/16530))可能发生，当if条件语句中的一行代码被注释，下一行代码会无意地成为if条件语句的一部分。另外，这种规范和其它条件语句保持一致，因此也更加容易阅读。

**推荐:**
```objc
if (!error) {
  return success;
}
```

**不推荐:**
```objc
if (!error)
  return success;
```

或者

```objc
if (!error) return success;
```

<a name="ternary-operator"></a>
### 三目操作符

当需要提高清晰性和代码整洁性，三目操作符`?:`应该才会被使用。常常使用在单个条件语句。多个条件语句时，如果使用`if`语句或重构成实例变量，代码会更加易读。一般情况下，根据条件来赋值，是三目操作符最好的使用场景。

`Non-boolean`变量应该和其它值进行比较，也应该加上`()`来提高可读性。如果被比较的变量是`boolean`类型，那么不需要添加`()`。

**推荐:**
```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

**不推荐:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

<a name="init-methods"></a>
## Init 方法

`Init` 方法应该遵循`Apple生成代码模板`的规范。返回类型也应该使用`instancetype`，而不是`id`。

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```

查看关于instancetype的文章:[Class Constructor Methods](#class-constructor-methods)

<a name="class-constructor-methods"></a>
## 类构造器方法

当类构造器方法被使用时，返回类型应该总是使用`instancetype`，而不是`id`。这样可以确保编译器正确地推断返回类型。

```objc
@interface Airplane
+ (instancetype)airplaneWithType:(MGSAirplaneType)type;
@end
```
关于instancetype的更多信息，请查看: [NSHipster.com](http://nshipster.com/instancetype/).

<a name="cgrect-functions"></a>
## CGRect函数

当访问一个`CGRect`的`x`, `y`, `width`或者`height`时，总是使用[CGGeometry]((http://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CGGeometry/Reference/reference.html))函数来访问, 而不是直接访问结构体成员。引自Apple的`CGGeometry`

> 在这个参考文档中的所有函数，接收CGRect结构体作为输入，在计算出它们结果之前隐式地标准化这些rectangles。由于这个原因，你的应用程序应该避免直接访问和修改保存在CGRect数据结构中的数据。相反，使用这些函数来操作rectangles和获取它们的特性。

**推荐:**

```objc
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
CGRect frame = CGRectMake(0.0, 0.0, width, height);
```

**不推荐:**

```objc
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
CGRect frame = (CGRect){ .origin = CGPointZero, .size = frame.size };
```

<a name="golden-path"></a>
## 黄金路径

当使用[条件语句](#conditionals)编码时，代码的左手边缘应该是“golden”或者"happy"路径。也就是说，不要嵌套`if`语句。多个return语句是OK的。

**推荐:**

```objc
- (void)someMethod {
  if (![someOther boolValue]) {
	return;
  }

  //Do something important
}
```

**不推荐:**

```objc
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

<a name="error-handling"></a>
## 错误处理

当方法通过引用返回一个错误参数，判断返回值而不是错误变量。

**推荐:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**不推荐:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

在成功的情况下，有些Apple的APIs存储垃圾值(garbage values)到错误参数(如果non-NULL)，那么判断错误值会导致false负值(随后crash)。

<a name="singletons"></a>
## 单例模式

单例对象应该使用线程安全模式来创建共享实例。

```objc
+ (instancetype)sharedInstance {
  static id sharedInstance = nil;

  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [[self alloc] init];
  });

  return sharedInstance;
}
```

这会防止:[possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html)。

<a name="xcode-project"></a>
## Xcode 工程

物理文件(夹)应该与Xcode工程文件(夹)保持同步来避免文件错乱。任何创建的Xcode分组都应该是文件系统中文件夹的体现。代码不仅可以根据类型来分组，而且也可以根据功能来分组，这样代码更加清晰。

尽可能在target的Build Settings中打开“Treat Warnings as Errors”和启用[additional warnings](http://boredzo.org/blog/archives/2009-11-07/warnings)。如果你需要忽略特殊的警告，使用[Clang's pragma feature](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas).

## 其它Objective-C编码规范

如果我们的代码规范不符合你的品位，可以看一些其它的编码规范:

* [Robots & Pencils](https://github.com/RobotsAndPencils/objective-c-style-guide)
* [New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)































