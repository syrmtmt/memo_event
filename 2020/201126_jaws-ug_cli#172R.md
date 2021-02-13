# JAWS-UG CLI専門支部 #172R S3基礎 (オブジェクト)

## 概要

* はじめてきた！
* <https://jawsug-cli.connpass.com/event/188843/>

---

## 事前準備

* はじめて来たのでCloud9環境作るところから・・・Cloud9も初めて触る
* はたのさん資料を見ながら黙々とやる。 <http://prototype-handson-cli.s3-website-ap-northeast-1.amazonaws.com/handson_light_web-aws_prepare/handson_light_web-aws_prepare-cloud9/index.html#id7>
* できた <http://prototype-handson-cli.s3-website-ap-northeast-1.amazonaws.com/handson_light_web-aws_prepare/handson_light_web-aws_prepare-cloud9/cloud9_web-environment-create-case-handson-cloud9-env.html>
* と思ったら`事前作業3. Cloud9作業ユーザーの作成` - `事前作業5. ハンズオン用権限ポリシーの作成`をやらないとだった

---

* 今日の資料 <http://prototype-handson-cli.s3-website-ap-northeast-1.amazonaws.com/handson_light-aws_service/handson_light-aws_service-s3_object/index.html>

（やっと追いついた？追いついてないか）

* なんか詰まってしまった <http://prototype-handson-cli.s3-website-ap-northeast-1.amazonaws.com/handson_light-aws_service/handson_light-aws_service-s3_object/iam-group-update-expand-policy-attach-local-S3BucketWrite-prefix-awsid-object_operation.html>

>[ec2-user@ip-10-0-0-119%]$ aws iam attach-group-policy \
>   --group-name ${IAM_GROUP_NAME} \
>   --policy-arn ${IAM_POLICY_ARN}
>  
>An error occurred (NoSuchEntity) when calling the AttachGroupPolicy operation: The group with name handson-cli-s3-MaintGroup cannot be found.

* このページをやり直したけど変わらず・・何が起きてるのか不明
    * あ、`事前作業`<http://prototype-handson-cli.s3-website-ap-northeast-1.amazonaws.com/handson_light-aws_service/handson_light-aws_service-s3_object/index.html#id7>一部しかやってなくね！

* 結局追いつかないまま終わったｗｗｗｗｗ

---

後日（12/2）本編2からのんびりやりました。

* （ここにきてやっとかよ感あるけど）認証情報設定してexportして、っていう手順はcloud9だからなのかな？
    * 今日かいしゃの人がやってたみたいな、ローカルにazure ps落として操作するみたいなのがAWS CLIにあたるって理解でいいんだろうか。
* 何回も環境変数設定したり・・ってのはそれぞれの操作を単独でやるときのためにってこと？それとも都度やんなきゃいけないってこと？

* 後処理始めようとしたらiamlistできないって言われちゃって謎すぎるのでiamのまわりは手作業で消していくことにした。 < 後始末1.1と1.2
* 後始末1.4でバケット消そうとしたらaccessdeniedだってよ。今誰でいじってんの。
    * この具合
    >[ec2-user@ip-10-0-0-119%]$ aws s3api delete-bucket \
--bucket ${S3_BUCKET_NAME}
An error occurred (AccessDenied) when calling the DeleteBucket operation: Access Denied \
[ec2-user@ip-10-0-0-119%]$ aws sts get-caller-identity
{
    "UserId": "xxxxx",
    "Account": "xxxxx",
    "Arn": "arn:aws:iam::xxxxx:user/handson-cli-s3-MaintUser"
}
    * s3-Mainsuserだって。 cf. <https://qiita.com/awesam86/items/7105e514b5d40f955337#json%E5%BD%A2%E5%BC%8F%E3%81%A7%E6%83%85%E5%A0%B1%E5%8F%96%E5%BE%97%E3%81%99%E3%82%8B>
    * えーそうなの？
    >{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ListObjectsInBucket",
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": [
                "arn:aws:s3:::handson-cli-s3-object-xxxxx"
            ]
        },
        {
            "Sid": "AllObjectActions",
            "Effect": "Allow",
            "Action": "s3:*Object",
            "Resource": [
                "arn:aws:s3:::handson-cli-s3-object-xxxxx/*"
            ]
        }
    ]
}
    * うーんバケット削除も手動か。

* 事後作業1.1は、、だめです。listができないもん。
* 事後作業1.3, 1.4, 1.5も不思議なエラーが出た。


http://prototype-handson-cli.s3-website-ap-northeast-1.amazonaws.com/handson_light_web-aws_prepare/handson_light_web-aws_prepare-cloud9/index.html#id7

http://prototype-handson-cli.s3-website-ap-northeast-1.amazonaws.com/handson_light-aws_service/handson_light-aws_service-s3_object/index.html

https://jawsug-cli.connpass.com/event/188843/