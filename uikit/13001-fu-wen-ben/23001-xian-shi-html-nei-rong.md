# 显示html文本



```
 NSAttributedString * attrStr = [[NSAttributedString alloc] initWithData:[chartMessage.content dataUsingEncoding:NSUnicodeStringEncoding] options:@{ NSDocumentTypeDocumentAttribute: NSHTMLTextDocumentType } documentAttributes:nil error:nil];
        
 self.chartView.contentShortLabel.attributedText = attrStr;
        
```



