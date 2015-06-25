# NSString

### 1. 创建字符串

```
NSString *str = @"hello 中国";  // 字面量方式创建，不需要自己释放内存
NSString *str2 = [[NSString alloc]init]; // 需要自己管理内存
```

### 2. 字符串长度

```
NSLog(@"NSString length=%lu",[name length]);
```

### 3. 比较字符串

```
[strA isEqual:strB]
if ([@"hello NSString" isEqualToString:str])
```

### 4. 拼接字符串

```
NSString *str = @"hello 中国";
NSString *str2 = [str stringByAppendingString:@", hello 世界"];
> str2 = 
hello 中国, hello 世界
```

### 5. 分割成数组 Separate

```
NSString *strs = @"xiaoqi,xiaodi";
NSArray *arr = [strs componentsSeparatedByString:@","];

> arr =
(
    xiaoqi,
    xiaodi
)
```
### 6. 数组组装成字符串 Join

```
NSArray *arr = @[@"xiaoqi",@"xiaodi"];
NSString *str = [arr componentsJoinedByString:@","];
> str = 
xiaoqi,xiaodi
```
### 7. C和 OC字符串转换

```
char *cStr = "I'm a C String";
NSString *ocString = @"I'm an OC String";

NSString *fromCharStr = [NSString stringWithUTF8String:cStr];
NSLog(@"%@", fromCharStr);

char *fromOCStr = [ocString UTF8String];
NSLog(@"%s", fromOCStr);

> 
I'm a C String
I'm an OC String
```

### 8. 大小写转换

```
NSString *upperStr = @"XIAODI";
NSString *lowerStr = [upperStr lowercaseString];
NSLog(@"%@", upperStr);
NSLog(@"%@", lowerStr);
> 
XIAODI
xiaodi
```

### 9. 格式化输出

```
NSString *name = 
	[NSString stringWithFormat:@"name=%@, age=%d", @"futeng", 20];
```


### 10. 判断前后缀

```
if (str hasPrefix:@"X")
if (str hasSuffix:@"X")
```
### 11. 比较字符串

```
# NSComparisonResult
```

### 12. 截取字符串；

```
 NSString *str = @"XIAODI";
 
 //local 起始点，length 长度
 NSRange range = NSMakeRange(2, 3);
 
 NSLog(@"%@", [str substringWithRange:range]);
 NSLog(@"%@", [str substringFromIndex:1]);
 NSLog(@"%@", [str substringToIndex:2]);
> 
AOD
IAODI
XI
```

### 13. 查找字串

```
 NSRange rangeFind = [str rangeOfString:@"中国"];
 NSLog(@"first location =%ld, range length=%ld", rangeFind.location, rangeFind.length);
> 
first location =6, range length=2
```

### 14. 替换字串；

```
NSString *str = @"hello 中国, hello FUTENG.ME";
NSLog(@"new slogan = %@", [str stringByReplacingCharactersInRange:NSMakeRange(0,5)
                                                               withString:@"你好"]);
> 
new slogan = 你好 中国, hello FUTENG.ME

NSLog(@"other slogan = %@", [str stringByReplacingOccurrencesOfString:@"hello"
                                                                   withString:@"你好"]);
>
other slogan = 你好 中国, 你好 FUTENG.ME
```
    
### 15. 读写文件

```
// 读取网络URL文件
NSURL *rfc2616 = 
	[NSURL URLWithString:@"https://www.ietf.org/rfc/rfc2616.txt"];

NSString *rfc2616res = [NSString stringWithContentsOfURL:rfc2616
                                               encoding:NSUTF8StringEncoding
                                                  error:nil];
// NSLog(@"url res = %@", rfc2616res);

// 读取本地文件
NSString *host = @"/etc/hosts";

NSString *fileRes = [NSString stringWithContentsOfFile:host encoding:NSUTF8StringEncoding error:nil];
NSLog(@"file res=%@", fileRes);

// 写入本地文件
BOOL isOK = [rfc2616res writeToFile:@"/tmp/rfc2616"
       atomically:YES
         encoding:NSUTF8StringEncoding
            error:nil];
if (isOK) {
    NSLog(@"OK");
}


NSString *rfc2612FromFile = 
	[NSString stringWithContentsOfFile:@"/tmp/rfc2616"
                 encoding:NSUTF8StringEncoding error:nil];
NSLog(@"read rfc2612 from file = %@", rfc2612FromFile);
```






