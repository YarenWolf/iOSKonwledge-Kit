# iOS应用数据存储技术

> * XML属性列表（plist）归档
> * Preference（偏好设置）
> * NSKeyedArchiver归档（NSCoding\)
> * SQLite3
> * Core Data

#### 

#### 

#### 

#### 应用沙盒包括4部分

应用程序包:MainBundle。不是文件夹，而是一个压缩包，包含所有的资源文件和可执行文件

Documents：保存应用运行时生成的需要持久化的数据。

Library:Preferences ：保存应用的偏好设置，itunes会备份；Caches：保存应用运行时生成的需要持久化的数据，一般存储体积较大，不需要备份的非重要数据

Tmp：临时数据，系统会清楚该目录下的文件

