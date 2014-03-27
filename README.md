iOSアプリ開発メモ
===============

## 基本テクニック

### NSLog無効化

これをprefixヘッダーに書いておくとReleaseビルドでログが出力されなくなる。

    // NSLog無効化
    #ifndef DEBUG
    # ifndef NSLog
    #  define NSLog( m, args... )
    # endif
    #endif

### GCDでバックグラウンド実行する書き方

    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        //--Background
     
        dispatch_async(dispatch_get_main_queue(), ^{
              //--Main Thread
        });
    });

### HTMLの16進カラーコードからUIColorを生成するマクロ

    #define HEXCOLOR(c) [UIColor colorWithRed:((c>>16)&0xFF)/255.0 green:((c>>8)&0xFF)/255.0 blue:(c&0xFF)/255.0 alpha:1.0]

マクロなので実行時オーバーヘッドを気にする必要が無いのが良い。


## iOS7 UI対応

### Foundationフレームワークの版を調べるコード

    if (floor(NSFoundationVersionNumber) <= NSFoundationVersionNumber_iOS_6_1) {
       // iOS 6.1以前に対応した処理
    } else {
       // iOS 7以降に対応した処理
    }

### レイアウトがずれる件

UIScrollViewおよび派生クラス（UITableView等）のスクロール上端がステータスバー分（20pt）下にずれる問題の解消。

    if (floor(NSFoundationVersionNumber) > NSFoundationVersionNumber_iOS_6_1) {
    	self.automaticallyAdjustsScrollViewInsets= NO;
    }

（[UIViewController viewDidLoad]内に記述）

### iOS7シミュレーターのみ、ナビゲーションバー絡みでレイアウトが崩れる件

デフォルト状態のナビゲーションバーを表示していると実機ではずれていないのにiOS7シミュレーターのみずれる。
以下で解消。

    #if TARGET_IPHONE_SIMULATOR
    	//--シミュレーター向けビルドした時のみ有効
    	if (floor(NSFoundationVersionNumber) > NSFoundationVersionNumber_iOS_6_1) {
    		self.navigationController.navigationBar.translucent= NO;
    	}
    #endif

（[UIViewController viewDidLoad]内に記述）


### iOS7ボタン文字色

青い文字。
rgb(31,128,235) (#1f80eb)


## iOSに限定されないもの

### シェルスクリプトで、自分自身の置かれているディレクトリをカレントにする

    #!/bin/sh
    cwd=`dirname $0`
    cd $cwd

