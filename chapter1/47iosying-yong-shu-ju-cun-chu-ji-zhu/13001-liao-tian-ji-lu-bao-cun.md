# 如何保存聊天记录

> 我们的聊天机器人需要保存用户和机器人的聊天记录，下次进来的时候发现本地有保存的上次的聊天记录就将聊天记录加载出来。也就是说当前需求是需要保存聊天记录
>
> 由于设计Model是多层嵌套所以普通保存需要归档，遵循NSCoding协议，层次较多修改起来很麻烦，所以想把Model直接保存下来，起初想着设计数据库但是比较麻烦，找到一个github库，可以直接将数据不用遵循NSCoding协议，然后转换成系统支持的可以保存的数据。

地址：[https://github.com/nicklockwood/FastCoding](https://github.com/nicklockwood/FastCoding "A faster and more flexible binary file format replacement for NSCoding, Property Lists and JSON")

用法：

* 导入FastCoder.h文件
* 使用我封装好的下面的办法

```

#import <Foundation/Foundation.h>
#import "FastCoder.h"

@interface XLChatSynchronizationAssistant : NSObject


+(void)saveChatRecords:(id)datas;


+(id)fetchChatRecords;


@end


#import "XLChatSynchronizationAssistant.h"

@implementation XLChatSynchronizationAssistant

+(void)saveChatRecords:(id)datas{
    NSArray *data = datas;
    NSArray *needsData = datas;
    if (data.count > 30) {
        needsData = [data subarrayWithRange:NSMakeRange(0, 30)];
    }
    
    NSString *path = [[[self class] documentsDirectory] stringByAppendingPathComponent:@"XLRobotChat.fast"];
    NSData *encodedData = [FastCoder dataWithRootObject:needsData];
    [encodedData writeToFile:path atomically:YES];
    
    
}


+(id)fetchChatRecords{
    NSString *path = [[self documentsDirectory] stringByAppendingPathComponent:@"XLRobotChat.fast"];
    NSData *data = [NSData dataWithContentsOfFile:path];
    return [FastCoder objectWithData:data];
}

+ (NSString *)documentsDirectory{
    return [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject];
}

@end


```



