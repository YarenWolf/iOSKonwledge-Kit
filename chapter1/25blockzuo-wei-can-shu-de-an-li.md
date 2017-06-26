# Block作为函数参数

> （1）设计一个类，该类具有一个方法对char \*\[\]数组的元素进行排序。那么如何实现？第一思路，先利用冒泡排序对数组元素进行排序
>
> （2）如果排序规则不要限定死，排序规则由函数调用者确定那么如何排序？这个规则千变万化，当然你也可以设计一个函数，该函数带个参数，参数类型是枚举类型，在函数内部根据枚举的值判断数组元素的排序规则。这样子也是不可取的，因为你始终无法穷举完所有的可能性
>
> （3）如果需要设计一个方法，该方法可以遍历数组，将数组每个遍历到的元素返回

```
//（1）
//LBPArray.h
#import <Foundation/Foundation.h>

@interface LBPArray : NSObject

@property (nonatomic, assign) int length;
//排序规则是字典顺序
-(void)sortWithCountries:(char *[])countries andLength:(int)len;

@end

//LBPArray.m
#import "LBPArray.h"

@implementation LBPArray

-(void)sortWithCountries:(char *[])countries andLength:(int)len{
    for(int i = 0; i<len; i++){
        for (int j = 0; j<i-1; j++) {
             //排序规则写死
            int res = strcmp(countries[j], countries[j+1]);
            if (res > 0) {
                char *temp = countries[j];
                countries[j] = countries[j+1];
                countries[j+1] = temp;
            }
        }
    }
}
@end


//test
char *countries[] = {"Nepl",
                        "Cambodia",
                        "Afghanistan",
                        "India",
                        "South Korea",
                        "Yuenan"};
    int length = sizeof(countries)/8;
    LBPArray *sorter = [LBPArray  new];
    //排序规则定死
    [sorter sortWithCountries:countries andLength:length];
    for(int i= 0;i<length;i++){
        NSLog(@"%s",countries[i]);
    }
```

```
//（2）
//LBPArray.h

#import <Foundation/Foundation.h>
typedef BOOL (^NewType)(char *country1,char *country2);
@interface LBPArray : NSObject

@property (nonatomic, assign) int length;
//排序规则是字典顺序
-(void)sortWithCountries:(char *[])countries andLength:(int)len;
//排序规则由调用者决定。
//那么如何实现？排序需要知道2个元素的比较结果，返回一个BooL值，也就是排序规则由调用者传递进来，该代码在函数内部执行
-(void)sortWithCountries:(char *[])countries andLength:(int)len andComareBlock:(NewType)compareBlock;

@end

//LBPArray.m
#import "LBPArray.h"

@implementation LBPArray

-(void)sortWithCountries:(char *[])countries andLength:(int)len andComareBlock:(NewType)compareBlock{
    self.length = len;
    for(int i = 0; i<len; i++){
        for (int j = 0; j<i-1; j++) {
            BOOL res = compareBlock(countries[j+1],countries[j]);
            if (res == YES) {
                char *temp = countries[j+1];
                countries[j+1] = countries[j];
                countries[j] = temp;
            }
        }
    }
}

-(void)sortWithCountries:(char *[])countries andLength:(int)len{
    for(int i = 0; i<len; i++){
        for (int j = 0; j<i-1; j++) {
             //排序规则写死
            int res = strcmp(countries[j], countries[j+1]);
            if (res > 0) {
                char *temp = countries[j];
                countries[j] = countries[j+1];
                countries[j+1] = temp;
            }
        }
    }
}

@end

//test
char *countries[] = {"Nepl",
                        "Cambodia",
                        "Afghanistan",
                        "India",
                        "South Korea",
                        "Yuenan"};
    int length = sizeof(countries)/8;
    LBPArray *sorter = [LBPArray  new];
    //排序规则由调用者决定
    //根据字符串长度排序
    [sorter sortWithCountries:countries andLength:length andComareBlock:^BOOL(char *country1, char *country2) {
        int res = (int)strlen(country1)- (int)strlen(country2);
        return res > 0 ;
    }];
```

```
//(3)
//LBPArray.h
#import <Foundation/Foundation.h>
typedef BOOL (^NewType)(char *country1,char *country2);
@interface LBPArray : NSObject

@property (nonatomic, assign) int length;
//排序规则是字典顺序
-(void)sortWithCountries:(char *[])countries andLength:(int)len;
//排序规则由调用者决定。
//那么如何实现？排序需要知道2个元素的比较结果，返回一个BooL值，也就是排序规则由调用者传递进来，该代码在函数内部执行
-(void)sortWithCountries:(char *[])countries andLength:(int)len andComareBlock:(NewType)compareBlock;

//遍历数组并返回
-(void)bianliCountry:(char *[])countries WithBlock:( void(^)(char * country))processBlock;

@end


//LBPArray.m
#import "LBPArray.h"

@implementation LBPArray

-(void)sortWithCountries:(char *[])countries andLength:(int)len andComareBlock:(NewType)compareBlock{
    self.length = len;
    for(int i = 0; i<len; i++){
        for (int j = 0; j<i-1; j++) {
            BOOL res = compareBlock(countries[j+1],countries[j]);
            if (res == YES) {
                char *temp = countries[j+1];
                countries[j+1] = countries[j];
                countries[j] = temp;
            }
        }
    }
}

-(void)sortWithCountries:(char *[])countries andLength:(int)len{
    for(int i = 0; i<len; i++){
        for (int j = 0; j<i-1; j++) {
             //排序规则写死
            int res = strcmp(countries[j], countries[j+1]);
            if (res > 0) {
                char *temp = countries[j];
                countries[j] = countries[j+1];
                countries[j+1] = temp;
            }
        }
    }
}

-(void)bianliCountry:(char *[])countries WithBlock:(void (^)(char *country))processBlock{
    for(int i=0;i<self.length;i++){
        processBlock(countries[i]);
    }
}


@end

//test
    char *countries[] = {"Nepl",
                        "Cambodia",
                        "Afghanistan",
                        "India",
                        "South Korea",
                        "Yuenan"};
    int length = sizeof(countries)/8;
    LBPArray *sorter = [LBPArray  new];
 [sorter bianliCountry:countries WithBlock:^(char *country) {
        NSLog(@"元素->%s",country);
    }];
```

* 什么时候Block可以作为方法的参数？

  * 当方法的内部需要执行一个功能，但是这个功能的具体实现在函数的内部不确定，这个时候调用者就用Block将方法实现的代码传进来
  * 

```
typedef void(^OurBlock)();
OurBlock test(){
    OurBlock block = ^{
        NSLog(@"你好");
    };
    return block;
}
```



