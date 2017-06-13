# ARC

1. ARC机制下当2个对象互相引用的时候如果2边都使用strong，那么就会造成内存泄漏  
   1. 解决方案：一端使用strong，另一端使用weak

2. ARC机制下如果属性的类型是OC对象类型绝大多数情况使用strong。

3. 在ARC机制下，如果属性的类型不是OC对象，使用assign

4. strong 和weak都是应用在属性的类型是OC对象的情况下，属性的类型不是OC对象则使用assign。

5. 在ARC机制下将MRC下的retain转换为ARC的strong

6. @property \(nonmatic ,strong \) Car \*car;代表生成的私有属性是1个强类型

7. @property \(nonmatic ,weak \) Car \*car;代表生成的私有属性是1个弱类型

8. 如果 对属性不写strong 和weak中的任一属性，默认就是strong（强类型）

9. ARC机制下对象什么时候被回收？

10. 指向对象的指针被回收

11. 指向对象指针被赋值为nil



