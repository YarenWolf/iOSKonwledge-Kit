# Block作为函数参数

> （1）设计一个类，该类具有一个方法对char \*\[\]数组的元素进行排序。那么如何实现？第一思路，先利用冒泡排序对数组元素进行排序
>
> （2）如果排序规则不要限定死，排序规则由函数调用者确定那么如何排序？这个规则千变万化，当然你也可以设计一个函数，该函数带个参数，参数类型是枚举类型，在函数内部根据枚举的值判断数组元素的排序规则。这样子也是不可取的，因为你始终无法穷举完所有的可能性

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

```

* 
* 
* 什么时候Block可以作为方法的参数？

  * 当方法的内部需要执行一个功能，但是这个功能的具体实现在函数的内部不确定，这个时候调用者就用Block将方法实现的代码传进来



