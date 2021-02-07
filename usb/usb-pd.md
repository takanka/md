# USB PD周りのメモ

自分が欲しいと思った充電器とかその辺りのメモ

## 使う機器
### MacBook Air
システムレポート見てる限りは100Wで充電できていそう

MacBook Proは[10Wでも充電できないこともないらしい](https://pelicanmemo.hatenablog.com/entry/2020/06/14/110000)

### iPad Pro
11inchでも[30W](https://ytanium.com/?p=11526)充電できる模様

### Surface Go
[30Wあれば安心な模様](https://shinoblogavi.wordpress.com/2019/02/20/surfacegoac/)

### スマホ系
大体が9A/3Aの18Wあれば事足りそう

## 充電器
CIOの充電器が手に入りやすくて高出力

| メーカー | 型番 | 価格 | port | 最大出力 |
| :---: | :---: | ---: | :---: | ---: |
| CIO | CIO-G100W3C1A | ¥7,678 | A1/C3 | 100W |

### 持ってる
- [CIO-G100W3C1A](https://connectinternationalone.co.jp/product/cio-g100w3c1a)  
    100W対応の充電器の中でType-Cが一番多いもののはず。利用中  
    [65W+30Wか45W+30W+18Wあたりで使える](https://prtimes.jp/main/html/rd/p/000000040.000043212.html)

## モバイルバッテリー
60W overなもの

| メーカー | 型番 | 価格 | port | 最大出力 | 最大接続パターン |
| :---: | :---: | ---: | :---: | :---: | :---: |
| ZENDURE | SuperTank Pro | ¥26,800 | C4 | 100W | 60W/60W/15W(2p) |
| Hyper | HyperJuice 2700mAh USB-C | ¥17,000 - ¥20,000 | A1/C2 | 100W | 合計130W |
| Anker | PowerCore III Elite 25600 87W | ¥10,490 | A2/C2 | 87W | 18W/45W/15W(2p) |

### 持ってる
- RavPower [RP-PB058](https://www.ravpower.jp/shop/battery/large/type-c-large/rp-pb058_bk)  
    結構昔に買った。合計出力が30Wなので力不足になりAnker 87Wにリプレース  
    [パススルーに対応しているみたい](https://car-accessory-news.com/rp-pb058/)なのでなにかに使うかも
- Anker [PowerCore III Elite 25600 87W](https://www.ankerjapan.com/item/A1291.html)  
    パススルー充電ができないのが少し惜しいけどコスパが良いのでいったんこれを使用

### 検討したもの
- ZENDURE [SuperTank Pro](https://zendure.com/products/supertank-pro?lang=ja)  
    唯一の100W Type-C 4portのはず。ただ高い
- Hyper [HyperJuice 27000mAh USB-C モバイルバッテリー](https://www.hyperjapan.jp/hp43002/)  
    2020年前半くらいまではこれくらいしか100W対応がなかった。パススルー対応してるしものは良さそうだけどポート数が足りない

## ケーブル
- 5A出力ケーブル
    - 3A overの出力を出すときは[eMarker](https://k-tai.watch.impress.co.jp/docs/column/keyword/1232948.html)が必須
    - 逆に[「3Aを超えないUSB Type-C スタンダードケーブルのUSB 2.0ケーブルを3Aで使う場合、eMarkerはオプションで、それでもUSB PDはサポートする」](https://pc.watch.impress.co.jp/docs/column/config/1071659.html)のでeMarkerなしのケーブルも当然たくさんある
    - eMarker付きとと無しのケーブルが混ざったときに外見でわからないのが厳しい。。。
- Apple系ケーブル
    - [古い機種で充電できない問題](https://hanpenblog.com/6924)
        - 高速充電 (USB PD) に対応していないiPhone・iPadで充電できない場合がある
    - [充電が進まない問題](https://hanpenblog.com/10458)
        - 充電中に見えても全然充電が進まない問題
        - 回避方法
            - まずiPhone・iPadにケーブルを挿す
            - そのあと、ケーブルをACアダプターに接続する