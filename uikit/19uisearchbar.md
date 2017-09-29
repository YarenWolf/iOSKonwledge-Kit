# UISearchBar

1、Xib拖出来的UISearchBar底部会有一个灰色，设置去掉灰色则需要设置一个空的图片但是问题又出来了，会显示SearchBar的ScopeBar，代码中需要设置showScopeBar=NO,scopeButtonTitles=nil.，如果需要去掉则需要做一些代码设置

```
self.searchBar.backgroundImage = [UIImage new];
    self.searchBar.showsScopeBar = NO;
    self.searchBar.scopeBarBackgroundImage = [UIImage new];
    self.searchBar.scopeButtonTitles = nil;

    switch (self.type) {
        case QuestionType_Science:
            self.searchBar.placeholder = @"输入搜索文章";
            break;
        case QuestionType_Error:
            self.searchBar.placeholder = @"输入搜索题目";
            break;
        case QuestionType_Missing:
            self.searchBar.placeholder = @"输入搜索题目";
            break;
        default:
            break;
    }

    UITextField *textField = [self.searchBar valueForKey:@"searchField"];
    if (textField) {
        textField.layer.cornerRadius = 14;
        textField.layer.masksToBounds = YES;
    }
```

2、纯代码使用UISearchBar

```
 self.sixthSearchBar = [[UISearchBar alloc] initWithFrame:CGRectMake(0, 390, UIScreenWidth, 40)];
    self.sixthSearchBar.backgroundImage = [UIImage new];
    UITextField *field = [self.sixthSearchBar valueForKey:@"searchField"];
    if (field) {
        field.layer.borderWidth = 1;
        field.layer.borderColor = [UIColor blackColor].CGColor;
        field.layer.cornerRadius = 13;
        field.layer.masksToBounds = YES;
    }

    self.sixthSearchBar.selectedScopeButtonIndex = 1;
    [self.sixthSearchBar setImage:[UIImage new] forSearchBarIcon:UISearchBarIconResultsList state:UIControlStateNormal];
    self.sixthSearchBar.searchBarStyle = UISearchBarStyleDefault;
    self.sixthSearchBar.showsScopeBar = YES;
    self.sixthSearchBar.scopeButtonTitles = @[@"已支付",@"待支付"];
    self.sixthSearchBar.scopeBarBackgroundImage = [UIImage imageNamed:@"nav_bar_ios7"];
    [self.sixthSearchBar setShowsCancelButton:YES animated:YES];
    self.sixthSearchBar.placeholder = @"请输入检索条件";
    self.sixthSearchBar.tintColor = [UIColor yellowColor];
    self.sixthSearchBar.delegate = self;
    [self.view addSubview:self.sixthSearchBar];
```

配合一些代码方法

```
#pragma mark -UISearchBarDelegate
- (void)searchBar:(UISearchBar *)searchBar selectedScopeButtonIndexDidChange:(NSInteger)selectedScope{
    NSLog(@"%zd",selectedScope);
}

-(void)searchBar:(UISearchBar *)searchBar textDidChange:(NSString *)searchText {
    NSLog(@"textDidChange->%@",searchText);
    if (searchBar == self.sixthSearchBar) {
        searchBar.showsBookmarkButton = YES;
    }
}

- (void)searchBarBookmarkButtonClicked:(UISearchBar *)searchBar{
    NSLog(@"点击了mark");
}

-(void)searchBar:(UISearchBar *)searchBar textDidChange:(NSString *)searchText {
    NSLog(@"textDidChange->%@",searchText);
}
```



UISearchBar加到NavigationItem上不显示光标

```
 
    UIView *titleView = [[UIView alloc] initWithFrame:CGRectMake(0, 3, BoundWidth-64, 32)];
    UISearchBar *searchBar = [[UISearchBar alloc] initWithFrame:CGRectMake(-20, 0, titleView.frame.size.width-40, 32)];
    searchBar.placeholder = @"搜索内容";
    searchBar.tintColor = [UIColor colorFromHexCode:@"426bf2"];   //不加不显示光标
    searchBar.delegate = self;
    searchBar.backgroundColor = [UIColor whiteColor];
    searchBar.layer.cornerRadius = 12;
    searchBar.layer.masksToBounds = YES;
    [searchBar.layer setBorderWidth:8];
    [searchBar.layer setBorderColor:[UIColor whiteColor].CGColor];
    self.searchBar = searchBar;

    [titleView addSubview:searchBar];
    self.navigationItem.titleView = titleView;
```



