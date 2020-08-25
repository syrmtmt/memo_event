# VMware DevOps Meetup #5

## 概要

---

## NSX-Tから見たvSphere with Kubernetesのネットワーキング

### vSphere with K8sとは

* vSphere with K8s = vSphereクラスタをk8sクラスタとして扱う
    * = k8sのAPIを通してESXiの上でPodを管理できる
* vSphere7.0とNSX-T3.0を使って構築します **（全然しらないけど7.0ってことはw/z k8sが新しめの機能なのね）**

### k8sのネットワーキング

* pod間のネットワーク
    * NSX-Tでいう「セグメント」
* service（エンドポイント）　：clusterIP（クラスタ内部との通信で使用される）,loadbalancer（クラスタ外部との通信で使用される）
    * NSX-Tでいう「分散ロードバランサ/ロードバランサ(L4 TCP)」
* ingress（L7ロードバランサ）
    * NSX-Tでいう「ロードバランサ(L7 HTTP)」
* network policy（ファイアウォール）
    * NSX-Tでいう「分散ファイアウォール」

### NSX-Tから見たvSphere with k8s

* spherelet/NCPを介してNSX - k8s(k8s API)間のやりとりがされる
* CIDRの設定が特殊なので注意
    * Pod/DLB/LB/SNATのIPをそれぞれ設定する必要がある
* vSphere with k8sの有効化やりかた
    * 各コンポーネントを用意／NSX Edgeには外部ネットワークと接するためのTier-0 GWを作成しておく
    * vSphere Clusterにcontrol plane VM*3台が作成される／Tier-0 GWの下にTier-1 GWが作成される／ESXiごとに（namespaceごとに）オーバーレイネットワークが作成される

---

## Ansible VMwareモジュールの今までとこれから

* 資料 <https://speakerdeck.com/sky_joker/ansible-vmwaremosiyurufalsejin-matetokorekara>

### AnsibleとCollectionsについて

* Ansible = IaCを実現するOSSツールのひとつ
    * yamlのコードで標準化された管理を行うことができる
* Collections = ansible-base以外のモジュールやプラグインのパッケージが外出しされたもの
    * バージョンも含めて選択できるようになるのは利用者からみてもハッピー！
    * 新しい概念:namespace = モジュール名が同じでサービスが違う場合に区別できる識別子
* Ansible 2.10は2020/9/22GAです
    * are

### さまざまなVMware関連Collections

* VMware Collections
    * vmware, vmware_rest（開発中）
    * vSphere 6.7.0+Zuul環境でCIしてます
    * ESXi無償版では使えません・・
        * 「個人で検証する場合は、60日評価版を使う or VMUG Advantageを購入してください」
    * 「vmware_restとvmwareは適宜使い分けてください」
* VMware REST Collection
    * vCenter 6.5から実装されたREST APIの仕様をもとに自動でモジュールが生成されるもの
    * Python3のみでしか動きません

### Ansible 2.9から2.10へ

* ansible_builtin_runtimeでcollectionsへのリダイレクトが設定されてます
（一時離席）
* が、FQCNを使うのが推奨です。

---

## Octant で k8s を手軽に可視化してみる

* vSpere with k8sk8sクラスタのうち、Tanzu K8sクラスタはk8sリソースを確認しにくいので、Octantのwebダッシュボードで可視化してみましたの回
* vSphere Clientだと、スーパバイザクラスタおよびその上のVMまでしか捉えられない（k8sリソースもほぼ操作できない）
    * Tanzu k8sクラスタはそのVMの上にPodを立てていくものなので、ここが可視化されたらいい
（デモ）

---

## Docker+Tensorflowの学習をBitfusionで高速化

* bitfusion server（仮想アプライアンス）をデプロイして、GPUとほげほげさせた（以下わからなすぎてメモを放棄）

---

## Japan VMUGとDevOps（仮）

* VMUG紹介