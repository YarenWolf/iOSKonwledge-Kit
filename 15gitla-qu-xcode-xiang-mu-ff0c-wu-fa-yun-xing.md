# 神坑



> 有次，从git仓库上clone了一个iOS项目到本地。打开后发现那个标志很奇怪，也不能选择在模拟器或者手机上运行，查找了资料后总结一下。





选择  Product---&gt;Edit Scheme,在打开的框中，找到对应的工程。点击左下方的“Manage Schemes”,点击打开的对话框右上角的“Autocreate Schemes Now”按钮后，会生成一个新的行，删除原来的行，然后选中新的行，点击“OK”。问题解决。

