# 利用 flex 实现垂直居中

> 父元素的 display 设置为 flex ，元素本身设置 margin 为 auto

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title></title>
        <style>
            div{
                height: 100px;
                background: brown;
                display: flex;
            }

            p{
                margin: auto;
            }
        </style>
    </head>
    <body>

        <div>
            <p>我是测试文字</p>
        </div>

    </body>
</html>
```

![](/assets/Flex-1.png)

