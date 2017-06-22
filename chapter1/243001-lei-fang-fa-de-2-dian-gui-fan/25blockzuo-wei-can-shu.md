# Block 作为参数



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



