###UIDatePicker
    时间选择器实时滚动并显示时间。

```
-(UIDatePicker *)datePicker{

 if (!_datePicker) {

 UIDatePicker *picker = [[UIDatePicker alloc] initWithFrame:CGRectMake(0, (BoundHeight-64)/2-105, BoundWidth, 210)];

 picker.datePickerMode = UIDatePickerModeDate;

 NSLocale *locale = [[NSLocale alloc] initWithLocaleIdentifier:@"zh_CN"];

 picker.locale = locale;

 NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];

 [dateFormatter setDateFormat:@"yyyy-MM-dd"];

 _datePicker = picker;

 }

 return _datePicker;

}

```

```
 [self.datePicker addTarget:self action:@selector(changeBirthdat) forControlEvents:UIControlEventValueChanged];
```

```
-(void)changeBirthdat{

 NSString *year = [[NSString stringWithFormat:@"%@",self.datePicker.date] substringToIndex:4];

 NSString *month = [[NSString stringWithFormat:@"%@",self.datePicker.date] substringWithRange:NSMakeRange(5, 2)];

 NSString *day = [[NSString stringWithFormat:@"%@",self.datePicker.date] substringWithRange:NSMakeRange(8, 2)];

 self.choosedLabel.text = [NSString stringWithFormat:@"%@年%@月%@号",year,month,day];

}

```