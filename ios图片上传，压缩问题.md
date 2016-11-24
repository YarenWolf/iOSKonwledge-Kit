###图片压缩问题

>https://segmentfault.com/q/1010000000701850



        IOS 拍照上传图片, 出错: 413 Request Entity Too Large. 一个图片也就3M大小。这个错误时由nginx报出来的, 请求被拦截,没有传递到springMVC 的后台, 可以通过查看nginx的日志, 看到这个错误。
服务器使用nginx作为反向代理服务器，是因为请求长度超过了nginx默认的缓存大小和最大客户端最大请求大小。

分别针对post和get方式的解决办法:

post

修改nginx.conf里面的几个相关的配置参数

client_body_buffer_size 10m(设置请求体缓存区大小, 不配的话)

client_max_body_size 20m(设置客户端请求体最大值)

client_body_temp_path /data/temp (设置临时文件存放路径。只有当上传的请求体超出缓存区大小时，才会写到临时文件中,注意临时路径要有写入权限)

如果上传文件大小超过client_max_body_size时，会报413 entity too large的错误。

get

我们可以通过修改另外两个配置来解决请求串超长的问题：

client_header_buffer_size 语法：

client_header_buffer_size size 默认值：1k 使用字段：http, server 这个指令指定客户端请求的http头部缓冲区大小绝大多数情况下一个头部请求的大小不会大于1k不过如果有 来自于wap客户端的较大的cookie它可能会大于1k，Nginx将分配给它一个更大的缓冲区，这个值可以在 large_client_header_buffers里面设置。

large_client_header_buffers 语法：

large_client_header_buffers number size 默认值：large_client_header_buffers 4 4k/8k 使用字段：http, server 指令指定客户端请求的一些比较大的头文件到缓冲区的最大值，如果一个请求的URI大小超过这个值，服务 器将返回一个”Request URI too large” (414)，同样，如果一个请求的头部字段大于这个值，服务器 将返回”Bad request” (400)。 缓冲区根据需求的不同是分开的。 默认一个缓冲区大小为操作系统中分页文件大小，通常是4k或8k，如果一个连接请求将状态转换为 keep-alive，这个缓冲区将被释放。

为什么修改http header的大小就能解决get请求串过长的问题？

因为get请求参数会拼在http header中，所以，修改了http header的大小，就能解决上面问题。

Nginx 400错误：HTTP头/Cookie过大

由于request header过大，通常是由于cookie中写入了较长的字符串所引起的。

解决方法是不要在cookie里记录过多数据，如果实在需要的话可以考虑调整在nginx.conf中的client_header_buffer_size(默认1k)

若cookie太大，可能还需要调整large_client_header_buffers(默认4k)，该参数说明如下：

请求行如果超过buffer，就会报HTTP 414错误(URI Too Long)

nginx接受最长的HTTP头部大小必须比其中一个buffer大，否则就会报400的HTTP错误(Bad Request)。



    ios图片压缩一般用UIImageJPEGRepresentation(newImage, 0.1)。但是此方法只能压缩到一定的大小，第二个参数不是压缩倍数。但是服务器如果给出最大限制的话我们需要自定义压缩方法。

```
 - (NSData *)imageWithImage:(UIImage*)image 
 scaledToSize:(CGSize)newSize{
 UIGraphicsBeginImageContext(newSize);
 [image drawInRect:CGRectMake(0,0,newSize.width,newSize.height)];
 UIImage* newImage = UIGraphicsGetImageFromCurrentImageContext();
 UIGraphicsEndImageContext();
 return UIImageJPEGRepresentation(newImage, 0.1);
}

```

    用法：例如我们从相册中选择了照片。点击上传的时候我们可以for循环给图片压缩，贴出代码
```
for (ALAsset *image in _mutlImageArray) {
 UIImage *tempImg = [UIImage imageWithCGImage:image.defaultRepresentation.fullScreenImage];
 NSData *imageData = [self imageWithImage:tempImg scaledToSize:CGSizeMake(tempImg.size.width , tempImg.size.height)];
 [_dataArray addObject:imageData];
 }
```

    