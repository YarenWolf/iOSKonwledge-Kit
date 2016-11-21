###ios view的frame和bounds之区别（位置和大小）

     一、首先列一下公认的资料： 

先看到下面的代码你肯定就明白了一些：

-(CGRect)frame{

 return CGRectMake(self.frame.origin.x,self.frame.origin.y,self.frame.size.width,self.frame.size.height);

}

-(CGRect)bounds{

 return CGRectMake(0,0,self.frame.size.width,self.frame.size.height);

}

很明显，bounds的原点是(0,0)点（就是view本身的坐标系统，默认永远都是0，0点，除非认为setbounds），而frame的原点却是任意的（相对于父视图中的坐标位置）。

