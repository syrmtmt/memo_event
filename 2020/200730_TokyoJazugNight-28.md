# 第28回 Tokyo Jazug Night (Online)

## 概要

* <https://jazug.connpass.com/event/181811/>
* Azureのuser group勉強会。
* live <https://www.youtube.com/watch?v=FhbbSWWC4NU>

---

## ARM版Windows Virtual Desktop事始め

* WVDとは　：Azureの仮想デスクトップサービス
    * v2(ARM版)が2020/7/27にGA！
        * 全部の管理がポータルでできるようになります。（いままで特定の管理がPowershellでしかできなかったりとかしてた）
        * **（ARMって言われると個人的にはプロセッサが最初に思いつくけど、Azureの世界ではAzure Resource Managerなのかそういうことなのか）**
    * Win10 Multi-sessionに唯一対応（Amazon WorkSpacesは対応してない）
        * Multi-sessionとは　：Semi-Annual Channel(SAC)で動作　**★なにこれ：というかいまだにマルチセッションの利点をあんまり理解してない**
        * Office365 ProPlusが快適に動くように最適化されている
    * AAD＋オンプレAD、ドメイン参加必須
    * 個人にVMを固定しない方法＝pooled, 固定する方法＝personal
    * ユーザプロファイル置き場はFSLogixが推奨
        * プロファイルをVHDファイルとしてマウントしている
        * これをAzure Blob StorageやAzure NetApp Filesに保管することもできる
    * サブネット設計はちゃんと考えてね／NSGは設定するのが推奨
* セキュリティの考慮が必要なところ
    * WVDのin/out通信制御
        * WVDに対する特殊な通信（セキュリティ）要件があるのであれば、別途、任意のセキュアなNW→AAD／WVDへの通信をNSGで許可しておく必要がある
        * 基本NSGで完結できるように→足りなければAzure Firewall→さらに足りなければ3rdpartyツールをば。
        * O365への通信を許可させたいなら、Application/Network rule collectionに365のIPレンジを書く必要があると・・・・ **自社サービスなんだからよきにやってくれや感**
    * ユーザ認証　：MFAやりましょうね
    * 脅威保護（エンドポイント含）　：Security Center, Defenderなど有効化しましょうね
    * モニタリング
        * 非アクティブユーザやアイドルセッションどうするかを決めとくとか
        * LogAnalyticsでいくつかのメトリックでログの収集・確認ができます **メトリック少なくね？標準メトリクスってそんなもん？**
        * Azure Sentinelとか使うとログ分析にいいですよ。ちょっとお金かかるけど。。

---

## DockerのAzure統合(ACI統合)について

* 「コンテナで動くようにしておくと、環境の再現性が高くて開発効率が上がるので、最近はDockedをよく使ってる。そうしてたらこないだDocker CLIがリリースされたので使ってみました。」
* Azure Container Instances(ACI)は、Dockerの実行環境として最高！
    * Docker Engineが入ったVMをpoolから貸出　：なのでVMを一から立てるよりプロビジョニングが速い！
    * hypervisorレベルでテナント分離されてます
    * Azure Files共有（永続的ストレージ）をマウント可
        * パフォーマンスの観点で必ずしも推奨するわけではないけど
* Azure Docker統合について
    * 6/25からACIを操作するのにdocker cliを使えるようになった！（本番環境を構築するならaz cliのほうが制限が少なくて良いは良いけど。）
    * azureにloginするところ以外はほぼdockerコマンドが使えます
    * （デモタイム）
    * ACI vs VMの価格比較　：VM*1.44=ACI　1.44倍なら許容範囲・・？
        * App Serviceだともう少しするが、その分色々入ってるのでね・・
    * 「起動が言うほど速くはないけどまあ速いほうじゃない」
    * 起動と停止がお手軽過ぎて、気づいたらazure上で普通に動かしてたりする
* **（そういえば大昔に一瞬Docker触りましたけど色々忘れましたね）（そういえばDCのGitlabもコンテナで動かしてましたっけ）**