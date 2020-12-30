## C45 知って楽しむルーティングセキュリティ

### ROVデプロイ状況調査

* by 渡辺 英一郎(ICT-ISAC Japan / NTTコミュニケーションズ株式会社)
* IXや大規模ISPによるROV実装報告が増えてきています
    * NTT GIN(AS2914)　：すべてのEBGPセッションで"invalid"判定された経路広告をreject
    * etc.
* isbgpsafeyet.comの判定方法（推定）
    * 自ASにおいてROVが実装されている場合
        * valid.cf.comへの接続=ok
        * invalid.cf.com(invalidホスト)への接続=ng
        * →"your AS is safe"
    * 実装されていない場合
        * ok
        * ok
        * →"unsafe"
    * 実装されていないが、経由するASのどこかで実装されている場合
        * ok
        * ng
        * →"safe"
* 日本初、IIJが実装したとのこと。 cf.<https://eng-blog.iij.ad.jp/archives/6861>
* JPNAPのルートサーバも昨日実装したとのことです！
* ROVが本当に実装されているのか実験。
    * KANDANET(38644) -- mf(7521) -- ...
    * ...
* NW運用者が考慮すべきこと
    * 経路のトラシュー時には、ROVの可能性も考慮する必要あり
    * 自身が管理するアドレスのROAを登録しましょう。その際は以下注意
        * punching holeの可能性はないか
        * max prefix lengthは適正か
        * ROA登録は定期的に正しく登録されているか

---

（あとはメモ取りを諦めたのかな・・）