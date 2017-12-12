# 常用富文本操作

```
switch (sender.tag - buttonTag) {
        case 0: //字体大小
        {
          [mAttStri addAttribute:NSFontAttributeName value:[UIFont fontWithName:@"微软雅黑" size:30] range:range];
        }
            break;
        case 1: //基线偏移值
        {
            [mAttStri addAttribute:NSBaselineOffsetAttributeName value:@10 range:range];
        }
            break;
        case 2: //字体颜色
        {
            [mAttStri addAttribute:NSForegroundColorAttributeName value:[UIColor redColor] range:range];
        }
            break;
        case 3: //背景颜色
        {
            [mAttStri addAttribute:NSBackgroundColorAttributeName value:[UIColor blueColor] range:range];
        }
            break;
        case 4: //字间距
        {
            [mAttStri addAttribute:NSKernAttributeName value:@20 range:range];
        }
            break;
        case 5:     //删除线
        {
            /*
             NSUnderlineStyleNone 不设置删除线
             NSUnderlineStyleSingle 设置删除线为细单实线
             NSUnderlineStyleThick 设置删除线为粗单实线
             NSUnderlineStyleDouble 设置删除线为细双实线
             */
            [mAttStri addAttribute:NSStrikethroughStyleAttributeName value:@1 range:range];
        }
            break;
        case 6:         //下划线
        {
            /*
             NSUnderlineStyleNone 不设置
             NSUnderlineStyleSingle 设置下划线为细单实线
             NSUnderlineStyleThick 设置下划线为粗单实线
             NSUnderlineStyleDouble 设置下划线为细双实线
             */
            [mAttStri addAttribute:NSUnderlineStyleAttributeName value:@1 range:range];
        }
            break;
        case 7:             //下划线颜色
        {
            [mAttStri addAttribute:NSUnderlineStyleAttributeName value:@1 range:range];
            [mAttStri addAttribute:NSUnderlineColorAttributeName  value:[UIColor greenColor] range:range];
        }
            break;
        case 8:         //删除线颜色
        {
            [mAttStri addAttribute:NSStrikethroughStyleAttributeName value:@1 range:range];
            [mAttStri addAttribute:NSStrikethroughColorAttributeName value:[UIColor orangeColor] range:range];
        }
            break;
        case 9:         //边缘线颜色
        {
            [mAttStri addAttribute:NSStrokeColorAttributeName value:[UIColor yellowColor] range:range];
            [mAttStri addAttribute:NSStrokeWidthAttributeName value:@0.2 range:range];
            
        }
            break;
        case 10:        //边缘线宽度
        {
            
            [mAttStri addAttribute:NSFontAttributeName value:[UIFont fontWithName:@"Helvetica" size:30] range:range];
            [mAttStri addAttribute:NSStrokeColorAttributeName value:[UIColor yellowColor] range:range];
            [mAttStri addAttribute:NSStrokeWidthAttributeName value:@0.8 range:range];
            
        }
            break;
        case 11:            //label阴影
        {
            NSShadow * shadow = [[NSShadow alloc]init];
            shadow.shadowBlurRadius = 5;//模糊度
            shadow.shadowColor = [UIColor orangeColor];//阴影颜色
            shadow.shadowOffset = CGSizeMake(1, 3);//偏移量
            [mAttStri addAttribute:NSShadowAttributeName value:shadow range:range];
            [mAttStri addAttribute:NSVerticalGlyphFormAttributeName value:@0 range:range];
        }
            break;
        case 12:                //横竖排版
        {
            [mAttStri addAttribute:NSVerticalGlyphFormAttributeName value:@1 range:range];
        }
            break;
        case 13:            //字体倾斜
        {
            [mAttStri addAttribute:NSObliquenessAttributeName value:@1 range:range];
        }
            break;
        case 14:            //文本扁平化
        {
            [mAttStri addAttribute:NSExpansionAttributeName value:@0.4 range:range];
        }
            break;
        case 15:            //url连接
        {
            [mAttStri addAttribute:NSLinkAttributeName value:[NSURL URLWithString:@"http://www.jianshu.com/p/6ce45e367519"] range:range];
        }
            break;
            
        default:
            break;
    }
    self.label.attributedText = mAttStri;
```



