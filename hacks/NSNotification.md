# NSNotification
​

### 获取通知中心引用

> 示例来自Stanford CS193p 2013-2014fall

```
[NSNotificationCenter defaultCenter];
```

此处调用的是通知中心Class方法，可以想象在你看不见的战阵迷雾里，你其实是获取到了iOS系统通知中心的一个默认引用，你可以借此做很多神奇的事情了。

### 收听

```
- (void)addObserver:(id)observer selector:(SEL)aSelector name:(NSString *)aName object:(id)anObject;

# 想成为收听系统的消息，你需要向通知中心发送addObserver相关消息；
# (id)observer  = 哪个instance来监听或者接收通知呢，而viewController就是个非常好的听众，所以很可能这里听众就是 self了；
# (SEL)aSelector  = 是啦，消息来了，处理这个消息的具体方法是什么呢，也就是需要制定一个SEL selector啦；
#  (NSString *)aName = 接收什么通知，比如 UIContentSizeCategoryDidChangeNotification，略长哇 - -；
# (id)anObject = 囧，不知道是什么，写个nil吧；；

# 示例

-(void) viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    [self usePreferredFonts];
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(preferredFontsChanged:)
                                                 name:UIContentSizeCategoryDidChangeNotification
                                               object:nil];
}


-(void) preferredFontsChanged:(NSNotification *) notification{
    [self usePreferredFonts];
}

-(void) usePreferredFonts {
    self.body.font = [UIFont preferredFontForTextStyle:UIFontTextStyleBody];//current system font
    self.headline.font = [UIFont preferredFontForTextStyle:UIFontTextStyleHeadline];
}
```

### 停止收听 tune out 

系统保持的这个observer引用不是week的，也就是如果你系统当前的ViewController不在当前页面，系统却不释放这个引用。此时一旦有通知来了，你app却处理不了，很可能就crash your app了。

```
-(void) viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    
    [[NSNotificationCenter defaultCenter] removeObserver:self
                                                    name:UIContentSizeCategoryDidChangeNotification
                                                  object:nil];
}
```