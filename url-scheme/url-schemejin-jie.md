###URL Schemes 的发展###


URL Schemes 的发展过程可以说就是 iOS 效率工具类 App 的发展过程。

起初的苹果建立的 Apple URL Schemes 只是用于自用，里面只有邮件、电话、iTunes 搜索、Youtube 视频等一些内置服务的 URL。

个人认为 URL Schemes 第一次大火是在 2011 年末（如有异议欢迎指正），那个时期也是越狱的鼎盛时期，那个时期越狱后大家都会装的一个插件是 SBSettings[1]。越狱的人都知道每当新系统发布的时候，等待新系统的越狱发布是最撩人的，而这段时期那些「不越狱就能做到某种越狱功能」的应用经常一时间风头无两。

2011年 iOS 5 发布带来了通知中心，没过多久，出现了一大批使用 iOS 系统设置的 URL Schemes 的 App 神奇地完成了接近 SBSettings 的功能——它们可以让我们从通知中心直接跳转到某些 App 的特定界面，比如 Twitter 的发推界面。它们甚至还可以直接跳转到系统设置里的 Wi-Fi 选项。在这一批 App 中，就有如今效率软件霸主之一 Launch Center Pro 的前身——Launch Center。



###基本 URL Schemes###

基本 URL Schemes 的能力虽然简单有限，但使用情境却是最普遍的。

我所谓的基本 URL Schemes，是指一个 URL 的 Schemes 部分，比如上文提到的微信的 weixin:。这个部分的唯一功能，就是打开相应应用，而不能够跳转到任何功能。

绝大多数所谓支持 URL Schemes 的应用，一般都是只有这么一个部分，它一般是这个应用的名称，比如 OmniFocus 这款应用，它的基本 URL Schemes 就是 Omnifocus:。如果应用的主名称是个中文名的话，它的 URL Schemes 也许会是应用名的拼音，比如 墨客 这款应用，它的基本 URL Schemes 是 moke:。

但，我前面提过了网页 URL 和 iOS 应用的 URL 的三个重要区别，其中第三项，就是 iOS 上的 URL Schemes 并不规范，一个应用的 URL 可以是各种各样的：
<ul>
<li>Coursera 的 URL 是：coursera-mobile:</li>
<li>Duet 这款游戏的 URL 是：x-kumo-duet:</li>
<li>Monument 这款游戏的 URL 是：fb690517270143345:</li>
<li>Feedly 的 URL 是：fb129765800430121:</li>
<li>扇贝新闻的 URL 是：wx95962d02b9c3e2f7:</li>
</ul>

它们目前并没有统一的规则，所以猜测一个应用的意义并不太大，你可以试试，但不要过于指望这种方式。如何查找一个应用的基本 URL Schemes，只要那个应用支持 URL Schemes 就能找到。


步骤
<ul>
<li>首先，在 iTunes 找到你想用 URL 打开的 App，右键选择在文件夹中显示：</li>
<li>然后解压该文件：</li>
<li>解压完毕后，在解压出的文件夹中，找到 .app 文件：</li>
<li>然后选择显示包内容：</li>
<li>找到 info.plist 这个文件，用你电脑里能打开它的 App 打开它（Xcode没得说）。</li>
<li>然后查找 URLSchemes：</li>
<li>在 CFBundleURLSchemes 下的那两行就是该 App 的基本 URL Schemes 了。</li>
</ul>

###复杂 URL Schemes###


参考链接：[URL Scheme](https://sspai.com/post/31500#fnref:2)




