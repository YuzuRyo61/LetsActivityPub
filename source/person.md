# ユーザー情報を読み取る

## 必要最低限の情報

最低限必要な情報は以下の通りである。

```type:json
{
    "@context": [
        "https://www.w3.org/ns/activitystreams",
        "https://w3id.org/security/v1"
    ],
    "id": "https://[このActivityPubのコードを表示するURL]",
    "type": "Person",
    "preferredUsername": "[username]",
    "inbox": "https://[このユーザーに対するInbox]",
    "publicKey": {
        "id": "https://[PublicKeyを識別できるID]",
        "owner": "[idと同じ]",
        "publicKeyPem": "-----BEGIN RSA PUBLIC KEY-----...-----END RSA PUBLIC KEY-----"
    },
    "url": "https://[プロフィールページのURL]",
    "manuallyApprovesFollowers": false
}
```

- type
  これはPersonもしくはServiceを指定する。
  Serviceを指定した場合、Mastodonではbotアカウントとして処理される。

- id
  以上のJSON形式を返すためのURL。

- preferredUsername
  ユーザーIDを代入する。半角英数字とアンダーバーを使用する（TwitterのユーザーIDと同じような形）。
  例：```letsAP_testuser```

- inbox
  外部からのデータを処理するInboxのURL。Inboxについては後に説明する。

- publicKey
  - id
    PublicKeyを識別するためのID。URL形式で処理する。
  - owner
    idと同じURLにする。（publicKey内のidではない）
  - publicKeyPem
    ユーザーを識別するための共有鍵を代入する。RSA形式で2048桁以上を推奨。
    共有鍵と秘密鍵のペアを作成しておく。これはInboxへ通信する際に必要である。

- url
  ユーザーのプロフィールを一般のブラウザで表示できるURLを代入する。

- manuallyApprovesFollowers
  フォローの許可・拒否を手動で行っているアカウントであるかを示す。

## 必要に応じて追加可能なもの

これらはアカウントの情報を拡張するものであり、必須ではない。

```type:json
{
    ...
    "outbox": "https://[このユーザーに対するOutbox]",
    "name": "Let's ActivityPub",
    "icon": {
        "type": "Image",
        "url": "https://[このユーザーのアイコン画像の直URL]",
        "sensitive": false
    },
    "image": {
        "type": "Image",
        "url": "http://[このユーザーのヘッダー画像の直URL]",
        "sensitive": false
    }
}
```

- outbox
  ユーザーの投稿を読み取るためのURL。

- name
  ユーザーの表示名。

- summary
  ユーザーのプロフィール。HTML使用可。

- icon
  - type
    Image固定。
  - url
    該当ユーザーのアイコン画像へリンクする直URL。
  - sensitive
    閲覧注意属性かどうかを指定。ただし、iconで使用する場合はfalseで良い。

- image
  iconと同じ内容だが、これは該当ユーザーのヘッダー画像を表示するものである。

> 現在執筆中です。しばしお待ちを。
