
# より理想のハイブリッドに近づいた？ 次世代 Azure Stack HCI とは？ [ウェビナー]

## 概要
* <https://info.microsoft.com/JA-AzureMig-WBNR-FY21-08Aug-24-AzureMigration-SRDEM36588_LP01Registration-ForminBody.html>

## 従来のAzure Stack HCIについて

* 入室遅れたので省略

## 新世代Azure Stack HCIについて

* HCI＝software defined storage+networking & 汎用x86サーバ
* 従来のAzure Stack HCI：Windows Server 2019ベース　→　新Azure Stack HCI：Azureベース
    * Azure Stack HCI専用のホストOSで動く（Windows Serverではない）
        * ドメコンやファイルサーバとして使うのはできなくなっていったりするかも。
        * ネットワーキング／リカバリ／ポリシーの従来提供している機能は変わらず利用できる
    * Azureの1サービスとして動く
        * ARMと連携：Azure Portalからあらゆる管理ができるようになっていく
        * Azureサポートの対象内になる
        * ホストOSの部分はAzureサブスクリプションに課金、ゲストOSは別途自分たちで準備
* 開発中の機能（20H2）について：preview中
    * Azure Stack HCIクラスタへのVM作成を、Azureディレクトリ内のユーザに委任できるように。
    * Azure Stack HCIの更新をAzure Portalから制御できるように。
    * DR構成を組めるように。
        * 離れた拠点にあるAzure Stack HCIどうしを1つのクラスタとして使用可能に。

## Azure Stackファミリー比較・おさらい

* Azure Stack Hub＝「Azureのオレオレリージョンを作るためのアプライアンス」
    * 要はハイブリッドクラウドを実現するためのプライベートクラウド基盤
    * 管理はAzure Stack Hub上のAzure Portal（パブリックなAzure Portalとは別物）から。
* Azure Stack Edge＝Edge computingの各ソリューションを利用可能
    * Edge Site（IoTデバイスセンサーとか）の利用に特化
    * 管理はパブリックなAzure Portalから。
* 新Azure Stack HCI＝Azure Portalをシングルコントロールプレーンとして使えるアプライアンス with Azure Arcのテクノロジー
    * 管理はパブリックなAzure Portalから。

* 続報は9月のIgniteで!!