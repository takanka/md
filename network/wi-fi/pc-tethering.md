# PCをWi-FiのAPとして使う
- ホテルやカフェでWi-Fi使いたいけど全部の保有機器で設定するのは面倒なのでノートPCをAPにして繋げられるようにしたい
- Bluetoothテザリングはいちいち選択しないと繋がらない＋早くないのでWi-Fiでつなぎたい

# WindowsでWi-Fiテザリング
- [記事](https://xtech.nikkei.com/atcl/nxt/column/18/00095/00022/)もあるくらい簡単
- 事前設定してたらタスクバーからモバイルホットスポットを選択するだけ

# MacでWi-Fiテザリング
パッと記事は見つからなかった記憶  
Macbookでは本体内臓のアンテナでWi-Fiを受けてさらにAPとして動作することはできなさそうなので下記の対応
- [Macに対応したUSBの子機](https://www.tp-link.com/jp/home-networking/adapter/archer-t2u-nano/)と[Type-C Plug to Type-A Receptacleの変換アダプタ](https://www.ainex.jp/products/u30ca-lfadt/)を組み合わせて2つめのWi-Fi接続を追加
    - ちなみに逆のType-C Receptacle to Type-A Plugの方は[仕様違反](https://hanpenblog.com/6148)
- インターネット共有で追加したEthernetアダプタを元にWi-Fiのデバイスで[共有](https://support.apple.com/ja-jp/guide/mac-help/mchlp1540/mac)
    - 上記とは逆のWi-Fiから追加したEthernetアダプタに共有するパターンはできない模様

## Macの課題
- 追加したEthernetアダプタで802.1X認証を設定する方法がわからないので[Secured Wi-Fi](https://support.ntt.com/ocn/support/pid2900000c7s)が共有できない