# NSMutableString


### 1. 创建字符串

```
NSMutableString *str = [[NSMutableString alloc] initWithString:@"hello,中国"];
NSLog(@"str = %@", str);
```

### 2. 追加，插入

```
[str appendString:@"; hello FUTENG.ME"];
[str appendFormat:@"; %d people likes;", 998];
[str insertString:@"; hello world" atIndex:25];
NSLog(@"str = %@", str);
```

### 3.  删除指定范围
```
NSRange range = NSMakeRange(22, 3);
[str deleteCharactersInRange:range];
NSLog(@"str = %@", str);
```

### 4.  查询范围
```
NSRange rangeOfChina = [str rangeOfString:@"中国"];
NSLog(@"find in %lu", rangeOfChina.location);
```

### 5. 改 替换
```
[str replaceCharactersInRange:rangeOfChina withString:@"China"];
NSLog(@"str = %@", str);

[str replaceOccurrencesOfString:@"hello"
                     withString:@"你好"
                        options:NSBackwardsSearch
                          range:NSMakeRange(0, 30)];

NSLog(@"str = %@", str);
```


