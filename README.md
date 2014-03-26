iOSアプリ開発メモ
===============

## iOS7 UI対応

### Foundationフレームワークの版を調べるコード

    if (floor(NSFoundationVersionNumber) <= NSFoundationVersionNumber_iOS_6_1) {
       // iOS 6.1以前に対応した処理
    } else {
       // iOS 7以降に対応した処理
    }

### レイアウトがずれる件

UIScrollViewおよび派生クラス（UITableView等）のスクロール上端がステータスバー分（20pt）下にずれる問題の解消

    if (floor(NSFoundationVersionNumber) > NSFoundationVersionNumber_iOS_6_1) {
    	self.automaticallyAdjustsScrollViewInsets= NO;
    }

（[UIViewController viewDidLoad]内に記述）

