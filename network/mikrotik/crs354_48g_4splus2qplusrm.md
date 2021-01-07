# CRS354-48G-4S+2Q+RM
公式： https://mikrotik.com/product/crs354_48g_4splus2qplusrm

レビューは [ServerTheHome](https://www.servethehome.com/mikrotik-crs354-48g-4s-2q-rm-review/) 辺りを参考にするといい

- なにがすごいか
    - いろいろ込みで約$600とやすいのに高収容
        - GbE 48Port
        - 10G SFP+ 4Port
        - 40G QSFP+ 2Port

[PoE付きのモデル](https://mikrotik.com/product/crs354_48p_4s_2q_rm)もあるけど必要としていないのでPoEなしの方で。

[EuroDK](https://www.eurodk.com/en/products/mikrotik)から2台輸入。

![Diagram](https://i.mt.lv/cdn/product_files/CRS354-48G-4Splus2Qplus_200122.png)
[公式](https://mikrotik.com/product/crs354_48g_4splus2qplusrm)のDiagram見てもわかるようにMarvellのchipの中で閉じないとCPUとの帯域が1Gbpsなのですっごい遅くなる。
(というのを誰かが書いていたのであとでそれをメモっておこう)

## SFPなど
おそらくどこのSFP/QSFPでも動きそう。ベンダーロックはされてなさそう

CRS354 2台の間の接続についてはfs.comの汎用QSFP+なAOCで今のところちゃんと動作している。買ったのは[このへん](https://www.fs.com/jp/products/74591.html?attribute=1696)

こんな感じに認識される
```
> /interface ethernet monitor qsfpplus2-1
                      name: qsfpplus2-1
                    status: link-ok
          auto-negotiation: done
                      rate: 40Gbps
               full-duplex: yes
           tx-flow-control: no
           rx-flow-control: no
               advertising: 
  link-partner-advertising: 
        sfp-module-present: yes
                  sfp-type: QSFP+
        sfp-connector-type: optical-pigtail
    sfp-link-length-copper: 10m
           sfp-vendor-name: FS
    sfp-vendor-part-number: QSFP-AO10
       sfp-vendor-revision: xxxx
         sfp-vendor-serial: xxxxxx
    sfp-manufacturing-date: YY-MM-DD
            sfp-wavelength: 850nm
           sfp-temperature: 28C
        sfp-supply-voltage: 3.259V
       sfp-tx-bias-current: 5mA
              sfp-tx-power: 0.233dBm
              sfp-rx-power: -5.083dBm
```

## qnap TS-453 Proとの相性(?)
ケーブル抜き差しのあと、L2ではLinkUPしていそうなんだけどパケットが通らないという事象が発生している。
InterfaceをDown/Upすれば治るのでこれは相性と割り切っている。

## 音的な意味で気をつけること
[ServerTheHome の Power Consumption and Noise のところ](https://www.servethehome.com/mikrotik-crs354-48g-4s-2q-rm-review/2/)では下記の通り電源を入れてすぐは静かになってトラフィックを流すと煩くなったとあるが、使ってみると動きとしては少し正確ではない
```
When we first plugged the switch in, the fans spun up, but then the switch fell silent. 
That is a bit misleading. When we ran traffic, the fans ramped up quite a bit.
```

- 電源を入れた当初はCPU温度が高くないのでFANが止まる
- なにもしなくてもCPU温度がすぐ上がるのでFANが回る
    - 見てる感じでは62℃くらいを超えると回る。アイドル状態でも温度は上がる
- そのFANがうるさい(約5,000-6,000rpm)
    - 隣の部屋に置いてもファンの音が聞こえる
- FANがまわってもCPU温度が下がらない
    - 写真を見たらわかるけどFANからCPUまでの距離があるのでかなりの冷気を当てないとCPUは冷えない
- 結果FANが回り続ける。うるさい

## 静穏化に必要な措置
結局はCPU温度に比例して回転数が上がるっぽい動きをしているのでCPUを極力冷やしつつ、FANがまわっても煩くならないようにする必要がある

- 標準の3つのFANを交換(MUST)
    - 各所で実績のある[NF-A4x20 FLX](https://noctua.at/en/nf-a4x20-flx)
    - [NF-A4x20 PWM](https://noctua.at/en/nf-a4x20-pwm)でもいいのでは？と思ってはいけない
        - PWMの方は一定のCPU温度になると回転数が0になってFAN Errorを検出する。おそらくPWMでコントロールされずに電圧で制御されてそう(なのでFLXのほうだとまともに動く)
        - 実用上問題になるかというと63℃くらいになってファンが回り50℃くらいで止まる(そして標準だとFAN Errorでランプが点灯する)を繰り返す程度なので問題にはならない。気分は良くない。
            - 当然設定でFAN Errorのときにランプをつけないこともできる
- CPUの近くに4つめのFANを追加(MUST)
    - NF-A4x20でも付けられるけど10mmの方が付けやすい、たぶん。スペースがなさすぎる
    - [N-FSTY-SMG](http://www.nagao-ss.co.jp/original53.html)で固定。FAN4のピンまでケーブル長が少し足りないけどNoctuaなら箱に延長ケーブルがあるので大丈夫
        - これ自身は2個1セットになっているけど20mmのFANをつけるなら片方だけしか実質つけられないので20mmつけるなら1セットで2台分まかなえる
- CPUの上にサーマルパッドを置く(OPTIONAL)
    - [TG-MP8-120-20-30-1R](https://www.shinwa-sangyo.co.jp/products/thermal-sheet/tg-mp8-120-20-30-1r)を4等分して重ねるとちょうど天板に張り付くくらいの厚みになる
    - ほんのり天板が暖かくなるが冷却という意味では効果はほぼない。気休め程度

ここまですると個人的には寝室に置いてても我慢できるレベルになった。部品交換してるから保証は無くなるけどそもそも安いし壊れたら買い替えくらいの気持ちで使うのが吉

## 何日かかる？
関西民なので成田じゃなくて関空経由。日曜注文・発送からの金曜到着なので約6日くらい

| 曜日 | 場所 |
| :---: | :---: |
| Sun | MARUPES LV |
| Mon | MARUPES LV |
| Tue | MARUPES LV | 
|     | RIGA LV |
| Wed | GRACE-HOLLOGNE BE |
|     | ROISSY CHARLES DE GAULLE CEDEX FR |
| Thu | NEW DELHI IN |
| Fri | GUANGZHOU CN |
|     | SENNAN-SHI JP(関空) |


## 結局いくらかかる？
EuroDKは他にいろんなものをつけた結果合計で$1,000くらいになったので金額は参考程度に。

| 型名 | 購入場所 | 個数 | 合計金額 |
| :--- | :---: | :---: | ---: |
| CRS354 | EuroDK | 2 | $763 |
| Power cord US C13 | EuroDK | 4 | $4 |
| Transport(FedEx Priority) | EuroDK | 1 | $98 |
| Insurance(保険つけた) | EuroDK | 1 | $10 |
| Bank fees(Paypal手数料?) | EuroDK | 1 | $41 |
| FedEx(関税) | - | 1 | ¥12,000 |
| N-FSTY-SMG(結果1つでよかった) | ヨドバシ | 2 | ¥1,834 |
| NF-A4x20 FLX | PCワンズ | 4 | ¥7,120 |
| NF-A4x20 PWM | PCワンズ | 4 | ¥7,120 |
| TG-MP8-120-20-30-1R | PCワンズ | 2 | ¥4,280 |

EuroDKで$916+¥12,000 -> ¥112,000($1 = ¥110として計算)
その他の静穏化グッズで¥20,000で合計約14万円

大体1台7万円とみればよさそう。

AmazonJPでも2台買った場合は¥52,183*2+¥6155 = ¥110,521なので大して変わらないはず。どっちにしろ同じEuroDKだし。
