# WebFinger

実装するにあたり、最初にWebFingerを用いる。

細かく説明するとわからないだろうと思うので砕いて説明するが、
これはActivityPubのプロフィール情報（PersonやServiceタイプ）を読み取るためのURLを置くものである。

Mastodonなどが連合のアカウントを検索するときにこのWebFingerというものを読み取り、
アカウントの有無を検知できる。

早速実装していこう。

## host-meta

```lang:xml
<?xml version="1.0" encoding="UTF-8"/>
<XRD xmlns="http://docs.oasis-open.org/ns/xri/xrd-1.0">
  <Link rel="lrdd" type="application/xrd+xml" template="https://[アドレス]/.well-known/webfinger?resource={uri}"/>
</XRD>
```

```[アドレス]```の部分にはサーバーを置くアドレスに置き換える。```{uri}```はそのままでよい。

このデータを```https://[アドレス]/.well-known/host-meta```にアクセスすると表示されるように処理する。

## WebFinger側の処理

URLのパラメーター、resourceに```acct:```から始まる形式が入る。例として

```acct:username@example.com```

という形だ。そしてこのパラメータがあるかつ、存在するユーザーの場合は以下のように返す。

```lang:json
{
    "subject": "acct:username@example.com",
    "links": [
        {
            "rel": "self",
            "type": "application/activity+json",
            "href": "https://[アドレス]/[該当ユーザーのActivityPubが返るURL]"
        }
    ]
}
```

最低でもこれでWebFingerは成り立つ。また、resourceパラメーターには小文字のみで処理される可能性もあるので、
ユーザーの存在の有無をチェックする際には大文字小文字の判別はしないことを勧める。

これを```/.well-known/webfinger```で読めるようにする。パラメータなしや不正な形式などの場合は404を出しても問題はない。

## 検証

[WebFingerの公式サイト](https://webfinger.net/)には簡易的な検証ツールがある。アクセスすると右上にフォームがあるので

```username@example.com```のような形式を入力して右の検索ボタンを押すと検証される。

結果が200で、「JSON Resource Descriptor」に以上のような結果が返ってくれば問題はない。

結果が404とかだったり、内容が違う場合はもう一度スクリプトを見直してみよう。
