# Objective-C - Coding Style
===

View
---
* 能坐就不要站, 若沒有模組化的考量請使用IB (storyboard 優先)
* 變數可使用`live render`來增加重用彈性
* Autolayout by code,推薦使用[Masonry](https://github.com/SnapKit/Masonry)

Model
---



*.h
---
//todo

*.m
---

#### 在@implementation底下 , 依順序分類寫
* Protocol寫全名 , 別人看code不熟悉時可以用command鍵去點它看內容
* method與method空一行
```objc

@implementation MYViewController

#pragma mark - life cycle
//例如viewDidLoad 依週期順序一直到 viewDidDisAppear ,如果你有需要用到

#pragma mark - UITableViewDelegate
//- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section

#pragma mark - UITableViewDataSource

#pragma mark - IBAction or event response
//- (void)viewDidTapped:(UITapGestureRecognizer*)tap

#pragma mark - public methods 

#pragma mark - private methods
//public跟private不見得要拆開,量少時分同一個也無仿

#pragma mark - getter & setter
//getter跟setter放這邊, 平常沒事不用看他

@end
```
* 上面這些你可以使用Xcode的code snippet來儲存樣板
* 其實照理說viewController不應該出現private method,你可以一開始就分幾個model來放，做別的case也能用
* controller盡量只做分配工作的角色

命名
---

#### Class名稱開頭大寫 , 物件名稱開頭小寫
* 命名盡量清楚並且有意義,除非是for迴圈裡面的i或j
* method名稱清楚明瞭可以省下寫註解的時間
* 名稱為 前綴名(非必需) + 名稱 + 類別名稱
```objc
ChatRoomListViewController *chatRoomListViewController;
```
#### 駝峰式命名
你應該使用
```objc
someOneViewController
```
而非
```objc
some_one_view_controller;
```
要不然像這樣就很可怕
```objc
some_one_view_controller.some_one_view.some_button.background_color;
```
一眼掃過去是分不出_跟.的


能使用點語法就不要用括號
---
你應該
```objc
[Class shareInstance].parameter;
```
而非
```objc
[[Class shareInstance] parameter];
```
這樣`方法`與`變數`的區分一目了然


Collections (Array & Dictionary )
---

#### 使用簡潔一點的語法

你應該使用
```objc
array[0][@"key"][@"key"];
```
而非
```objc
[[[array objectOfIndex:0] valueForKey:@"key"] valueForKey:@"key"]  ;
```

#### 物件化

* 更好的方式是使用[JSONModel](https://github.com/icanzilb/JSONModel)(推薦)或其他方式來包裝collections

使用
```objc
user.posts[0].title;
```
取代
```objc
user["post"][0][@"title"];
```
這樣就不會有寫錯key的困擾,尤其是撈API時 (你注意到posts沒加s了嗎?)

* 並且可以增加方法
```objc
[user.posts[0] saveToDB];
```

Getter
---
#### 需要initial的變數一律使用property
```objc
@property (strong, nonatomic) NSArray *array;
```
並且覆寫getter
```objc
- (NSArray*)array{
	if(!array){
		array = [[NSArray alloc]init];
	}	
	return array;
}
```

如此你將得到乾淨的 viewDidLoad:
也不用為了生命週期的錯誤耗費人生
避免出現以下狀況
```objc
- (void)viewDidLoad{
    [super viewDidLoad];

    self.textLabel = [[UILabel alloc] init];
    self.textLabel.textColor = [UIColor blackColor];
    self.textLabel ... ...
    self.textLabel ... ...
    self.textLabel ... ...
    [self.view addSubview:self.textLabel];
	
	===下略300行分隔線===
}
```
總之請把這些東西放在getter


Setter
---
//todo



Block
---
* method多傳入值時你可以折行
例如
```objc
    UIAlertView *alertView = [[UIAlertView alloc] initWithTitle:@"ERROR"
                                                        message:@"alertView exmaple"
                                                       delegate:nil
                                              cancelButtonTitle:@"Cancel"
                                              otherButtonTitles:nil];

```

* 但是有block時請避免
	例如
```objc
    [network connectWithHTTPMethod:CRUDMethod_POST URLString:urlString IDString:nil parameters:parameter alertWithError:YES success:^(AFHTTPRequestOperation *operation, id response) {
        cell.userInteractionEnabled = YES;
        sender.lock = NO;
        leader.user.isFollowed = !leader.user.isFollowed;
        sender.following = leader.user.isFollowed;
        
        dataSource[index.row] = [leader dictionaryRepresentation];
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
        cell.userInteractionEnabled = YES;
        sender.lock = NO;
    }];
```
而非
```objc
    [network connectWithHTTPMethod:CRUDMethod_POST
                         URLString:urlString
                          IDString:nil parameters:parameter
                    alertWithError:YES
                           success:^(AFHTTPRequestOperation *operation, id response) {
        cell.userInteractionEnabled = YES;
        sender.lock = NO;
        leader.user.isFollowed = !leader.user.isFollowed;
        sender.following = leader.user.isFollowed;

        dataSource[index.row] = [leader dictionaryRepresentation];
    } failure:^(AFHTTPRequestOperation *operation, NSError *error) {
        cell.userInteractionEnabled = YES;
        sender.lock = NO;
    }];
```

enum是好東西
---
記狀態非常好看
```objc
typedef NS_ENUM (unsigned int, LOMethod) {
    LOMethodGET,
    LOMethodPOST,
    LOMethodDELETE,
    LOMethodPATCH,
    LOMethodPUT
};
```

	

其他細節上的排版
---
* `if`與`()`該不該空格 , `=`跟左右兩側該不該空格 , 這不應該耗費時間來維護,請使用XCode外掛
[BBUncrustifyPlugin-Xcode](https://github.com/benoitsan/BBUncrustifyPlugin-Xcode)


最後
---
* `規則就是用來修改與打破的，目的是增加效率與減少錯誤，若有更好的見解請拿出來一起討論。`







