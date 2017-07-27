# Block 作为方法参数---高阶函数？

block 可以作为方法的参数，也可以作为方法的返回值

被作为参数传入的函数就叫做回调函数

```
#import <Foundation/Foundation.h>

int globalA = 10;
typedef int(^NewType) (int ,int);

int testWithBlock(NewType block){
    globalA++;
    int ele = globalA;
    int sum = block(ele,globalA);

    return sum + ele + globalA;
}


int main(int argc, const char * argv[]) {


        int sum = testWithBlock(^int(int a, int b) {
            return a - b;
        });

    NSLog(@"sum->%d",sum);


    return 0;
}
```

如何为一个函数指定不同的处理逻辑，鉴于Block的特点，可以用Block作为函数参数传递到函数内部去执行，因此该函数在调用的过程中调用者可以传递Block代码去在函数内部调用。

举例

```
#import <Foundation/Foundation.h>
#import "LBPArray.h"

int globalA = 10;
typedef int(^NewType) (int ,int);

int testWithBlock(NewType block){
    globalA++;
    int ele = globalA;
    int sum = block(ele,globalA);

    return sum + ele + globalA;
}

int testTheBlock(int(^myBlock)(int a,int b)){
    globalA++;
    int temp = globalA;
    int sum = myBlock(globalA,temp);
    return globalA + temp  + sum; 
}

int main(int argc, const char * argv[]) {

    int sum = testTheBlock(^int(int a, int b) {
        return a + b;
    });

    int minus = testTheBlock(^int(int a, int b) {
        return a - b;
    });

    int multiple = testTheBlock(^int(int a, int b) {
        return a * b;
    });

    int divide = testTheBlock(^int(int a, int b) {
        return a/b;
    });

    NSLog(@"sum->%d, minus->%d, multiple->%d, divide->%d",sum,minus,multiple,divide);

    return 0;
}
```



