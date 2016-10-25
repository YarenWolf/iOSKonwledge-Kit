###PrefixHeader.h文件

    1、新建文件ios->other->PCH file
    2、选中项目设置，TAEGETS->build settings->查找pch->Apple LLVM 8.0-Language->Precompile Prefix Header改为YES，Prefix Header后面跟pch在项目中的具体路径（eg:KSGuidViewDemo/PrefixHeader.pch）

/Users/geek/Desktop/YHPatientFrameWork/YHPatientFrameWork/SupportingFiles/PrefixHeader.pch-》
$(SRCROOT)/YHPatientFrameWork/SupportingFiles/PrefixHeader.pch


