# KVC

* 首先判断模型中查找是否有set属性名的方法（setAge），如果有就直接调用setAge:\(NSString \*\)age
* 继续在模型中查找是否有属性（age），如果有就直接访问成员属性
* 继续在模型中查找有没有\_属性（\_\_age），如果有就直接调用
* 如果都找不到则直接报错 \[属性名 setValue: forUndefinedKey\];

```
#import <Foundation/Foundation.h>

@interface Person : NSObject

@property (nonatomic, strong) NSString *name;
@property (nonatomic, assign) NSInteger age;

-(instancetype)initWithDic:(NSDictionary *)dic;
@end
#import "Person.h"

@implementation Person

-(instancetype)initWithDic:(NSDictionary *)dic{
    if (self = [super init]) {
        [self setValuesForKeysWithDictionary:dic];
    }
    return self;
}

-(void)setAge:(NSString *)age{
    _age = [age integerValue];
}

@end

//test
NSArray *array = @[@{@"name":@"lbp",@"age":@"22"},@{@"name":@"lbp",@"age":@"23"},@{@"name":@"lbp",@"age":@"24"},@{@"name":@"lbp",@"age":@"25"},@{@"name":@"lbp",@"age":@"26"}];



    NSMutableArray *arrays = [NSMutableArray array];
    for (int i= 0; i<array.count; i++) {
        Person *person = [[Person alloc] initWithDic:array[i]];
        [arrays addObject:person];
    }


    [arrays enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
        NSLog(@"%@%zd",obj,idx);
    }];
```



