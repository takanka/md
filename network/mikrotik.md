Mikrotikにかんするメモ

# 設定回り
Cisco慣れしている人はかなり勝手が違うのでつらい(体験談)。

# CRS354-48G-4S+2Q+RM
https://mikrotik.com/product/crs354_48g_4splus2qplusrm
レビューは https://www.servethehome.com/mikrotik-crs354-48g-4s-2q-rm-review/ 辺りを参考にするといい。

- なにがすごい
    - いろいろ込みで約$500とやすいのに高収容
        - GbE 48Port
        - 10G SFP+ 4Port
        - 40G QSFP+ 2Port

[PoE付きのモデル]{https://mikrotik.com/product/crs354_48p_4s_2q_rm} もあるけど必要としていないのでPoEなしの方で。

[EuroDK](https://www.eurodk.com/en/products/mikrotik)から輸入。

## qnap TS-453 Proとの相性(?)
ケーブル抜き差しのあと、L2ではLinkUPしていそうなんだけどパケットが通らないという事象が発生している。
InterfaceをDown/Upすれば治るのでこれは相性と割り切っている。

## 気をつけること
- 音的な意味でデフォルト構成でつかえると思うのはデータセンターの住人くらい
    - CPU温度がすぐ上がるのでFANは回る
        - 見てる感じでは62℃くらいを超えると回る
    - そのFANがうるさい(約5,000-6,000rpm)
        - 隣の部屋に置いてもファンの音が聞こえる
    - FANがまわってもCPU温度が下がらない
        - 結果FANが回り続ける
        - 写真を見たらわかるけどFANからCPUまでの距離があるのでかなりの冷気を当てないとCPUは冷えない

## 静穏化に必要な措置
- 標準の3つのFANを交換
    - 各所で実績のある[NF-A4x20 FLX](https://noctua.at/en/nf-a4x20-flx)
    - [NF-A4x20 PWM](https://noctua.at/en/nf-a4x20-pwm)でもいいのでは？と思ってはいけない
        - PWMの方は一定の電圧になると回転数が0になってFAN Errorを検出する。おそらくFAN1-3はPWMでコントロールされていない
        - FAN4だけはうまく動いてくれるようには見える
        - 実用上問題になるかというと63℃になってファンが回り50℃くらいで止まる(そして標準だとFAN Errorランプが点灯する)、を繰り返す程度なので問題にはならない

- CPUの近くに4つめのFANを追加
    - NF-A4x20でもいいんだけど10mmの方が付けやすい。スペース上の問題。
    - [N-FSTY-SMG](http://www.nagao-ss.co.jp/original53.html)で固定。FAN cableの長さが少し足りないけどNoctuaなら箱に延長があるので大丈夫

- CPUの上にサーマルパッドを置く
    - [TG-MP8-120-20-30-1R](https://www.shinwa-sangyo.co.jp/products/thermal-sheet/tg-mp8-120-20-30-1r)を4等分して重ねるとちょうど天板に張り付くくらいの厚みになる。

ここまですると個人的には寝室に置いてても我慢できるレベルになった