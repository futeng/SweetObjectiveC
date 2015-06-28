# NSAttributedString & NSMutableAttributedString

# 文本属性

文本的属性（或者样式），就是用来定义文字在界面如何呈现。

文本的属性包括：字体，颜色，大小，**加粗**，*斜体*，下划线，删除线，角标，字据等等，一切为了如何更优雅的呈现文字。

（上一句文字中，你一定看到了某几个字样式发生了变化。）

本文讨论的`NSAttributedString` 或 `NSMutableAttributedString` 就是完整存储文本及其属性的厉害家伙。

**那么问题来了，如何设计一个数据结构来存储这些内容呢？**

# 如何设计

### 结构

偷偷告诉你，真正牛逼的学霸是拿到试题就知道出题老师想考什么，扫一遍试卷就大致清楚这张卷子自己能考多少分。

这里面价值百万的思想是，你每站高一个层次来看待问题，理解也会更深一层次。

（比站在出题人角度思考更高的可能是站在市场需求的角度思考，于是你发现学这些到最后还不是跟前后位一个样，突出不到哪去，还不如……如果你被这一句话就怂恿的就想辍学，那说明你还太稚嫩了，继续迭代你的思想。已跑偏 - -）

那如果是你，该如何设计能存储文本格式的数据结构：


其实核心需求就两点嘛：

1. 字符串本身；
2. 每个字符都有自己的样式；

所以这个结构肯定需要一个存储普通字符串的指针，毋庸置疑。

样式最直觉的想法是想办法存储每个字符，及其所涉及的所有属性，但考虑到一个字符极大可能包裹这多个属性，比如同时加粗和高亮。想想就很麻烦（IndexArray(AttributeNameArray)貌似可以，不过够恶心的)。

**此时脑袋一定不能空白，一定不能空白，一定不能空白……**

你一定要感觉到自己是很聪明的，很聪明的继续往下思考了，比如以字符的角度存储不方便，那以**样式**的角度呢？（人之所以为人！！！）

于是有下面一个简单的设计：

```
@interface SimpleAttributedString

@property NSString *string;
@property NSDictionary *dictionary;

@end
```

1. 属性 string用来存储字符串本身；
2. dictionary用来存储，每种样式及其运用的范围；

回到本文第二行的，一共有55个字符，18-20范围的字符被加粗，21-23范围的字符被设置为斜体；

```
# 伪代码，不要在意这些语法细节了

NSString *str = @"文本的属性包括：字体，颜色，大小，**加粗**，*斜体*，下划线，删除线，角标，字据等等，一切为了如何更优雅的呈现文字。";

NSDictionary *dictionary = [[NSDictionary alloc] initWithAttributesName: 
              @{
                  @"BOLD":(18,20), 
                  @"ITALIC":(21-23)
              }];

SimpleAttributedString *sentence = [[SimpleAttributedString alloc] initWithString:str andDictionary:dict];
```

###方法

方法都是为了数据服务的。

那文本属性这样一个结构就很简单了，只需要考虑如何：

1. 设置字符串和获取字符串；
2. 设置属性值和获取属性值；

典型的bean和setter、getter，很简单的，还可以定义其=其他围绕这两个数据的增删该查，此处就不展开了；

# NSAttributedString & NSMutableAttributedString

经过上述一番探讨，再回到正题你会发现后面这些东西似乎都是**显然**的嘛。

1. `NSAttributedString.h`中确实存储了字符串和属性（样式）字典；
2. 属性字典的key是一个个预定义的属性名字符串，类似`NSForegroundColorAttributeName`表示前景色；
3. `NSMutableAttributedString` 是`NSAttributedString.h`的可修改子类实现；
4. 最后还是明确下这个结构定义的是字符样式，具体屏幕再来根据属性字典里面的描述渲染和呈现出效果；
5. 另外常用的`NSTextStorage`也是其可变子类；

（后面似乎都不好玩了呢）

### 常用方法

##### 1. 初始化属性字符串
```
- (instancetype)initWithString:(NSString *)str;
- (instancetype)initWithString:(NSString *)str attributes:(NSDictionary *)attrs;
- (instancetype)initWithAttributedString:(NSAttributedString *)attrStr;
```

##### 2. 获取指定字符的属性
```
- (id)attribute:(NSString *)attrName atIndex:(NSUInteger)location effectiveRange:(NSRangePointer)range;

# 求属性类型从指定location位置算起，一次性能匹配的多少个字符（范围）
# 注意只匹配一次，当次是尽可能多的匹配。

NSAttributedString *str = [[NSAttributedString alloc]initWithString:@" hello,**中国**"];
# 上述两个星号示意加粗中间的文字有前景色，于是这个方法可用为 从第0位开始数起，向右开始匹配，能一直匹配到多少个有前景色的字符（获得一个范围指针，你后面可以拿来用）

NSRange range;
id = [str attribute:NSForegroundColorAttributeName atIndex:0 effectiveRange:&range];

#如果匹配到则id不为0，否则id为0；
# range记录了匹配到的范围
NSMutableAttributedString *characters = [[NSAttributedString alloc]initWithString:@"你好,"];
[characters appendAttributedString:[str attributedSubstringFromRange:range]];

# characters = 你好,**中国**            
```

##### 3. 增加 删除 替换

```
- (void)addAttribute:(NSString *)name value:(id)value range:(NSRange)range;
- (void)addAttributes:(NSDictionary *)attrs range:(NSRange)range;
- (void)removeAttribute:(NSString *)name range:(NSRange)range;

- (void)replaceCharactersInRange:(NSRange)range withAttributedString:(NSAttributedString *)attrString;
- (void)insertAttributedString:(NSAttributedString *)attrString atIndex:(NSUInteger)loc;
- (void)appendAttributedString:(NSAttributedString *)attrString;
- (void)deleteCharactersInRange:(NSRange)range;
- (void)setAttributedString:(NSAttributedString *)attrString;
```

# 常见属性常量


No.|  Attribute Name                | Value Class | Explain
---|--------------------------------|-------------|---------
1  | NSForegroundColorAttributeName | NSColor     | 文字颜色
2  | NSBackgroundColorAttributeName | NSColor     | 文字背景色
3  | NSBaselineOffsetAttributeName  | NSNumber    | 行距
4  | NSUnderlineStyleAttributeName  | NSNumber    | 下划线
5  | NSStrokeWidthAttributeName     | NSNumber    | 文字边
    
# 课外阅读


[官方必读 UIKit Framework Reference  NSAttributedString UIKit Additions Reference - developer.apple](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/NSAttributedString_UIKit_Additions/index.html)



