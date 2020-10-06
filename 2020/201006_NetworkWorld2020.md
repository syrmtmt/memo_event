# Network World 2020

## ShowNetの構築と東京大学SD-WANの開発から見る、相互接続性と本当に必要だったもの

### SRv6 service chaining

* 2017-2018年は、IP経路制御：BGP Flowspecなどでサービスチェイニングをやってたりした。
* そして2019年はsource routing：SRv6
    * SRv6 proxyでSIDのセクションの取り外しを行う：End.AM　→でも"SRv6 over v4"をやらないといけないケースも出てくる。ヤバイ。
    * でも結局「1台で複数の取り外し挙動ができるSRv6 Proxy」が必要だったので、実装した。＝End.AC
    * End.ATと名前を変えてIETF draftが出ました。

### 東京大学SD-WAN

* SINET -- 本郷 --<Internet>--他の地方拠点へ・・
* VXLAN over Wireguardで実現＋経路制御は自動化　：L2 Encapされたパケットが運ばれてくるみたいなイメージ？

### まとめ
* 「API（インターフェース）が同じなら、内側の実装は変えてもよくない？」　★この考え方好き
    * ex. サーバ・ネットワーク機器とAnsible・chef・puppet
* （わたしはL2がなにもわからない）

---

## SDxでグローバルネットワークはどう進化するか

* gartner「2024年までにSD-WANの適用が60%になる」などなど、SDN化が進んでくるとの見立て。
* 一般的な拠点構成　：Spoke拠点 --<Private WAN>--Hub拠点 --<Internet>-- SaaS
    * これまでHub拠点にあったデータがSaaSに移っていく流れ
    * これからは　：Spoke拠点 --<SD-WAN Overlay>-- Hub拠点やSaaS　＋SD-WANを中央で集中管理
        * アプリケーションを識別して、アプリケーションごとのトラフィックルーティングも容易になる。
        * →各拠点の機器などは、調整した経路次第ではダウングレードも可能では。
（途中退席）