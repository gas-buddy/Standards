# GasBuddy Objective-C Style Guide.

## Table of Contents

* [Language](#language)
* [Localization](#localization)
* [Project Structure](#project-structure)
* [Code Organization](#code-organization)
* [Spacing](#spacing)
* [Comments](#comments)
* [Naming](#naming)
* [Underscores](#underscores)
* [Methods](#methods)
* [Variables](#variables)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Literals](#literals)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Case Statements](#case-statements)
* [Private Properties](#private-properties)
* [Booleans](#booleans)
* [Conditionals](#conditionals)
* [Ternary Operator](#ternary-operator)
* [Init Methods](#init-methods)
* [Class Constructor Methods](#class-constructor-methods)
* [Golden Path](#golden-path)
* [Error handling](#error-handling)
* [Singletons](#singletons)
* [Line Breaks](#line-breaks)


## Language

US English is favoured. Avoid our Canadian words such as; colour, grey, fuelled, and kilometre. 

**Acceptable:**
```objc
UIColor *grayColor = [UIColor grayColor];
```

**Unacceptable:**
```objc
UIColor *greyColour = [UIColor grayColor];
```

## Localization

All user facing text should reference a localized string. (Logs, errors, event labels, and analytics, should never be localized.)

Use GBLocalizedString macro (Objective-C) or the equivalent function (swift) in favour of NSLocalizedString as it removes the need for a "" parameters and reduces code. 

Localized strings are catagorized by their primary screen view controller. 

## Project Structure

Our project structure is generated. This assures all source files group locations in xcode mirror their physical location. 

This also pervents .pbxproj merge conflicts. 

Cocoapods are used when possible. Pods are commited into our repository. 

New classes should be created in Swift. The exception is a new cell of an existing Objective-C Table View. 

All inspectable elements exposed to Interface Builder exist in the GBDesignables Framwork. This allows very fast compile time when using custom interface builder objects.  

All classes are prefixed with GB. 


## Code Organization

Use `#pragma mark -` to categorize methods in functional groupings and protocol/delegate implementations following this general structure.

```objc
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}

#pragma mark - Memory

- (void)dealloc {}

- (void)didReceiveMemoryWarning {}
```

## Spacing

* GasBuddy uses 4 spaces. 4 spaces was a legacy decision. Spaces are esier to copy and paste into other projects than tabs. Opening brackets should be on the same line.

**Acceptable:**
```objc
if (object.property) {
    // Code
} else {
    // Code
}
```

**Unacceptable:**
```objc
if (object.property)
{
 //Do something
}
else {
 //Do something else
}
```

* Use two lines between methods. Whitespace within methods should separate functionality, but often there should probably be new methods.

* Colon-aligning method invocation should often be avoided.  There are cases where a method signature may have >= 3 colons and colon-aligning makes the code more readable. Please do **NOT** however colon align methods containing blocks because Xcode's indenting makes it illegible.

**Preferred:**

```objc
// blocks are easily readable
[UIView animateWithDuration:1.0 animations:^{
  // something
} completion:^(BOOL finished) {
  // something
}];
```

**Not Preferred:**

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

## Comments

A comment can / should be used to explain why. 

The method name should explain what. 

Comments used was warnings I.E. Update This:, Fix Me:, Refactor This:, should NEVER be used. Don't merge a pull request with a Fix Me. Future devs won't know the intention of the "Fix Me" and its often assumed because things are "working" it's best just to leave it alone.

Commented out code should never be committed to the repository in any main line.

Comments should include a space after the // and use proper grammar. 

```objc
// This is an example comment.  
```

## Naming

As per Apples standards. Use descriptive method and variable names. Avoid hungarian notation.

**Preferred:**

```objc
UIButton *settingsButton;
```

**Not Preferred:**

```objc
UIButton *setBut;
```

## Methods

Methods should not contain more than three parameters. Objects should be used to pass data.

The usage of the word "and" is reserved. 

Please use effort to keep methods small (Less than 30 lines)

**Preferred:**
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
- (void)sendAction:(SEL)aSelector to:(id)anObject forAllCells:(BOOL)flag;
- (id)viewWithTag:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width height:(CGFloat)height;
```

**Not Preferred:**

```objc
-(void)setT:(NSString *)text i:(UIImage *)image;
- (void)sendAction:(SEL)aSelector :(id)anObject :(BOOL)flag;
- (id)taggedView:(NSInteger)tag;
- (instancetype)initWithWidth:(CGFloat)width andHeight:(CGFloat)height;
- (instancetype)initWith:(int)width and:(int)height;  // Never do this.
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Always use camelCase. 

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants.



**Preferred:**

```objc
@interface GBTutorial : NSObject

@property (strong, nonatomic) NSString *tutorialName;

@end
```

**Not Preferred:**

```objc
@interface GBTutorial : NSObject {
  NSString *tutorialName;
}
```


## Property Attributes

Property attributes should be explicitly listed, and will help new programmers when reading the code.  The order of properties should be storage then atomicity, which is consistent with automatically generated code when connecting UI elements from Interface Builder.

**Preferred:**

```objc
@property (weak, nonatomic) IBOutlet UIView *containerView;
@property (strong, nonatomic) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (nonatomic, weak) IBOutlet UIView *containerView;
@property (nonatomic) NSString *tutorialName;
```

**Preferred:**

```objc
@property (copy, nonatomic) NSString *tutorialName;
```

**Not Preferred:**

```objc
@property (strong, nonatomic) NSString *tutorialName;
```

## Dot-Notation Syntax

Use Dot-notation for accessing and mutating properties. It makes code more concise. Bracket notation is preferred in all other instances.

**Preferred:**
```objc
NSInteger arrayCount = [self.array count];
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not Preferred:**
```objc
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. 

**Preferred:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone": @"Kate", @"iPad": @"Kamal", @"Mobile Web": @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingStreetNumber = @10018;
```

**Not Preferred:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingStreetNumber = [NSNumber numberWithInteger:10018];
```

## Constants

Constants are preferred over in-line string literals or numbers. (Breaking the avoid hungarian notation rule) Our constants are prefixed with kGB. 

**Preferred:**

```objc
#define kGBHostUrl @"https://support.gasbuddy.com"
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`

This also makes Objective-C enums more accessible in swift. 

**For Example:**

```objc
typedef NS_ENUM(NSInteger, GBLeftMenuTopItemType) {
  GBLeftMenuTopItemMain,
  GBLeftMenuTopItemShows,
  GBLeftMenuTopItemSchedule
};
```

You can also make explicit value assignments (showing older k-style constant definition):

```objc
typedef NS_ENUM(NSInteger, GBGlobalConstants) {
  GBPinSizeMin = 1,
  GBPinSizeMax = 5,
  GBPinCountMin = 100,
  GBPinCountMax = 500,
};
```

Older k-style constant definitions should be **avoided** unless writing CoreFoundation C code (unlikely).

**Not Preferred:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```

## Case Statements

Braces are not required for case statements, unless enforced by the complier.  
When a case contains more than one line, braces should be added.

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

Avoid fall-throughs where possible.  

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

## Booleans

NOTE: Objective-C uses `YES` and `NO`. Swift uses `true` and `false`

In Objective-C `YES` is defined as 1 and a `BOOL` can be up to 8 bits.

Comparing YES against a bit value greater than 1 results in false. 

Not a serious issue. Just worth noting.



## Conditionals

Conditional bodies should always use braces.

**Preferred:**
```objc
if (!error) {
  return success;
}
```

**Not Preferred:**
```objc
if (!error)
  return success;
```

or

```objc
if (!error) return success;
```

### Ternary Operator

The Ternary operator, `?:` , should only be used when it increases clarity or code neatness.

**Preferred:**
```objc
NSInteger value = 5;
result = (value != 0) ? x : y;

BOOL isHorizontal = YES;
result = isHorizontal ? x : y;
```

**Not Preferred:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Init Methods

Init methods should follow the convention provided by Apple's generated code template.  A return type of 'instancetype' should also be used instead of 'id'.

```objc
- (instancetype)init {
  self = [super init];
  if (self) {
    // ...
  }
  return self;
}
```

See [Class Constructor Methods](#class-constructor-methods) for link to article on instancetype.

## Class Constructor Methods

Where class constructor methods are used, these should always return type of 'instancetype' and never 'id'. This ensures the compiler correctly infers the result type. 

```objc
@interface Airplane
+ (instancetype)airplaneWithType:(GBAirplaneType)type;
@end
```

More information on instancetype can be found on [NSHipster.com](http://nshipster.com/instancetype/).


## Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path.  That is, don't nest `if` statements.  Multiple return statements are OK.

**Preferred:**

```objc
- (void)someMethod {
  if (![someOther boolValue]) {
	return;
  }

  //Do something important
}
```

**Not Preferred:**

```objc
- (void)someMethod {
  if ([someOther boolValue]) {
    //Do something important
  }
}
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**Preferred:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**Not Preferred:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).


## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.

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

This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).


## Line Breaks

Avoid breaking code onto multiple lines.

