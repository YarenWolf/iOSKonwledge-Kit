# 错误传播

## 实验一

```
<script>
'use strict';

function main(s) {
    console.log("begin main()...");
    try {
        bar(s);
    } catch (e) {
        console.log("main error is " + e);
    }
    console.log("end main()...");
}

function bar(s) {
    console.log("begin bar()...");
    try {
        show(s);
    } catch (e) {
        console.log("bar error is " + e);
    }
    console.log("end bar()...");
}

function show(s) {
    console.log("begin show()...");
    console.log("the length of s is " + s.toString().length);
    console.log("end show()...");
}

main(null);
</script>
```

结果

```
index.html:17 begin main()...
index.html:27 begin bar()...
index.html:37 begin show()...
index.html:31 bar error is TypeError: Cannot read property 'toString' of null
index.html:33 end bar()...
index.html:23 end main()...
content.js:390 undefined
```

我们可以看出在函数调用的时候顺序为: main\(\) -&gt; bar\(\) -&gt; show\(\) 

但是在 show\(\) 函数内部发生了错误，函数内部抛出错误，错误传递顺序为: show\(\) -&gt; bar\(\) -&gt; main\(\)



## 实验二

上面的代码稍微改一下

```
<script>
'use strict';

function main(s) {
    console.log("begin main()...");
    try {
        bar(s);
    } catch (e) {
        console.log("main error is " + e);
    }
    console.log("end main()...");
}

function bar(s) {
    console.log("begin bar()...");
    try {
        show(s);
    } catch (e) {
        console.log("bar error is " + e);
    }
    console.log("end bar()...");
}

function show(s) {
    console.log("begin show()...");
    try {
        console.log("the length of s is " + s.toString().length);
    } catch (e) {
        console.log("show error is " + e);
    }
    console.log("end show()...");
}

main(null);
</script> 
```

结果是

```
index.html:17 begin main()...
index.html:27 begin bar()...
index.html:37 begin show()...
index.html:41 show error is TypeError: Cannot read property 'toString' of null
index.html:43 end show()...
index.html:33 end bar()...
index.html:23 end main()...
content.js:390 undefined
```

结论

和实验一唯一不同的是在 show\(\) 函数内部加了异常捕获，看到 console 中异常是在 show\(\) 函数内部被捕获了。





## 实验三

在实验二的基础上对代码稍做修改

```
<script>
'use strict';

function main(s) {
    console.log("begin main()...");
    try {
        bar(s);
    } catch (e) {
        console.log("main error is " + e);
    }
    console.log("end main()...");
}

function bar(s) {
    console.log("begin bar()...");
    show(s);
    console.log("end bar()...");
}

function show(s) {
    console.log("begin show()...");
    console.log("the length of s is " + s.toString().length);
    console.log("end show()...");
}

main(null);
</script>
```

结果

```
begin main()...
index.html:27 begin bar()...
index.html:33 begin show()...
index.html:21 main error is TypeError: Cannot read property 'toString' of null
index.html:23 end main()...
content.js:390 undefined
```





### 敲黑板



如果在一个函数内部发生了错误，它自身没有捕获，错误就会被跑道外层调用函数，如果外层调用函数也没有捕获，该错误会一直沿着调用链向上抛出，直到被 Javascript 引擎捕获，代码终止执行





# 异步错误处理



```
<script>
'use strict';

function printError() {
    throw new Error();
}

try {
    setTimeout(printError, 1000);
    console.log("done");
} catch (e) {
    console.log("error-> " + e);
}
</script>
```

异步错误必须在函数内部处理错误外，外层代码无法捕获。

