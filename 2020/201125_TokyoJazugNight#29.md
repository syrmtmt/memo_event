# 第29回 Tokyo Jazug Night (Online)

## 概要

* 概要 <https://jazug.connpass.com/event/194512/>
* 配信 <https://www.youtube.com/watch?v=u_oh1xxbFZw>

---

## Windows Virtual Desktop 構築にまつわるアレコレ

* by Dai さん
* WVDの環境を、ADも含めて完全に新規で構築したおはなし。
    * 「過去、契約やERでハメられたことを考えると・・」**やっぱハマるんだER・・・**
* 全体構成の検討中に気がついたこと
    * シングルセッションで大量にセッションホストを作成する必要があった　→マルチセッションは便利だが、使用アプリの検証をする必要がある（そのための時間が必要）
        * マルチセッションをやって導入予定のアプリが動くか分からず、検証する必要があった
        * 稼働開始日が後ろに倒せなかった
    * FSLogixのプロファイル格納にAzure NetApp Filesを使うことにしたが・・・シングルセッションだとホストの台数がえぐいので、IPアドレス制限（最大1000IP）が足かせになる。　→NetApp Filesのvolume毎にvnetを分ければどうにかなるとのことで、ホストプールごとにvnetを分割することに。
    * SNATポート枯渇するんじゃね？　→「Azure FWでIPを確保（自主規制）し、色々と組み合わせてSNATポート利用数を抑えることでまず対処／間に合わない場合は手動拡張を依頼・・というスキームにしました」
        * 各セッションホストからのインターネットアクセスあり／EXOも使う／接続元IPで縛ってるシステムもあるから、ランダムにもしきれない
    * セッションホストを本番で作り始めると、201台目で止まる
        * WVDのホストプールには基本的に可用性セットが組まれる→ホストプールの数をさらに細分化して逃げることに・・・
        * Powershellでゴニョると可用性セットなしでも組めるらしいですが未確認です
    * AzureポータルやARMテンプレートで指定したドメイン参加用のID/PWが各セッションホストのローカル管理者になる　→セッションホストのドメイン参加用ID/PWは用意しないとだめ
    * セッションホストの電源管理、難しい。
        * 「そのあたりはCitrixやVMwareのほうが簡単にできるけどお金かかるしね・・」
* やってみてまとめ
    * vnet peering大活躍・・
    * オンプレADの方が結果手っ取り早い
    * GPOはなんだかんだ大活躍
    * WVDのRDPプロパティ設定はコロコロ変わるので・・・
* 注意点って結局？
    * 公式docや先人の知恵はちゃんと参考にしましょう
    * マルチセッションをできる限り活用してホストプールのVM台数を減らせるかが肝
    * 余裕をもった構築計画をば
    * 「Azure各リソースにおける点の知識をいかにして線や面として結び付けられるか」

---

## Azure ArcでSQL Server Anywhere

* by Masayuki_Ozawa さん
* （めちゃめちゃ聞きやすい）

* Azure Arc Enabled SQL Server <https://docs.microsoft.com/ja-jp/sql/sql-server/azure-arc/overview?view=sql-server-ver15>
    * オンプレSQLサーバをAzureポータル上で管理できるようにする機能
    * セキュリティ評価・対策もできる（SQL Assessment）
    * 「Azure Arc Enabled Serverのアドオンみたいな感じ」 <https://docs.microsoft.com/ja-jp/azure/azure-arc/servers/overview>
* Azure Arc Enabled Data Service <https://docs.microsoft.com/ja-jp/azure/azure-arc/data/overview>
    * k8s（データコントローラが動く）が動いている環境でSQL Managed Instance/Postgle Hyperscaleをデプロイすることができる
    * 現状、展開しているのをアップグレードしたいときは作り直しになります <https://docs.microsoft.com/ja-jp/azure/azure-arc/data/release-notes#known-limitations-and-issues>
    * Azure Data Studioを使えば簡単にデプロイできます
        * Visual Studio CodeのExtentionもAzure Data Studioで使えますよ
* （わからない言葉祭りなので途中からメモ取りを諦めてた）

---

## App ServiceやWeb App for Containersによる今どきのナウいリリースについて(Blue-Greenデプロイとかカナリアリリースとか)

* by noriyukitakeiさん
* cf. <https://tech-lab.sios.jp/archives/22822>
* App ServiceやWeb App for Containersを活用すると、Blue-Greenデプロイ／カナリアデプロイが簡単にできるようになりますよ
* スロットをスワップすることで本番／ステージングの切替ができる
    * スワップするときは、引き継がれる／引き継がれない設定をしっかり確認する必要があります

---

## Azure Functions をAzure 以外のクラウドプロバイダーで動かしてみる。（仮）

* by Jun Kudoさん
* Azure Functions Core ToolsをAWS EKSに乗せる。
* （わからない単語祭り）

---

## 市民開発者でもAzureを使いこなしてステキなアプリを作ろう！

* by Ryota Nakamuraさん
* Azureっていっぱいサービスがあるけど、市民開発者にとってはそんな簡単じゃない・・・・・でも「私も使ってみたい・・・」
* Power Platformを使えば、ローコードでアプリを作れるよ！
* ツイッターで検索したツイートの感情分析をするアプリを作ってみたよ！
    * Power Appsの画面でコネクタ繋いでやるだけ。
* 「弊社はPower AppsでWVDの制御とかもやってます」**マジか！！やりたい！！！！**

---

## bicep (ARM Template DSL)の紹介

* by Takekazu Omiさん
* bicep <https://github.com/Azure/bicep>
* （わからない単語祭り）