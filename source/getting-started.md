# はじめに

まず、ActivityPubのことについて簡単に説明しよう。

ActivityPubとはW3C勧告のプロトコルであり、例として、分散型SNS Mastodonでは1.6.0から対応した。

なお、このドキュメントはMastodonなどのSNSで使用できる形式で説明していく。

ActivityPubには投稿などを受け取るためのInbox、投稿を読み取るためのOutboxがある。

ActivityPubの通信は、JSON形式で行われる。以下が形式例である。

```json
{
    "@context": "https://www.w3.org/ns/activitystreams",
    "type": "Person",
    "id": "https://ap.example.com/user/username",
    "name": "Let's ActivityPub",
    "preferredUsername": "username",
    "summary": "It's a description!",
    "inbox": "https://ap.example.com/user/username/inbox",
    "outbox": "https://ap.example.com/user/username/outbox",
    "following": "https://ap.example.com/user/username/following",
    "followers": "https://ap.example.com/user/username/followers",
    "url": "https://ap.example.com/@username",
    "manuallyApprovesFollowers": false
}
```

※今回は見やすさを重視するために改行を施して記載している。

1つずつ見てみよう。

- @context

  これはActivityPubの通信である、というものを表すものであり、テキスト形式もしくはリスト形式で展開されている。ActivityPub通信の上では必須。

  なお、以下の形式(URL)が主に使用されることが多い。それに加えて独自の宣言もあるシステムもある。

  ```json
  [
      "https://www.w3.org/ns/activitystreams",
      "https://w3id.org/security/v1"
  ]
  ```

- type

  このActivityPubの形式は〇〇である、を定義するものであり、ActivityPub通信の上では必須。

  今回はPersonなので、ユーザーの形式で処理する。なおServiceも同じ形式であるが、Mastodon上などではbot扱いになる。

- id

  URL形式。そのURLにアクセスかつ、HTTPのAcceptヘッダーに```application/ld+json; profile="https://www.w3.org/ns/activitystreams"```や```application/activity+json```を付けてGETすると以上の形式が返る。

- name

  ユーザーの表示名。

- preferredUsername

  ユーザーのID。Mastodonなどではこれを使用してユーザーのIDとして使用する。

- summary

  ユーザーの説明。紹介文。HTML形式を表記できる。

- inbox

  受信ボックス。このURLに投稿の作成やいいねなどが送られてくる。

- outbox

  送信ボックス。このURLにアクセスすると、投稿の作成などを返るようになる。

- following

  フォロー情報のURL。

- followers

  フォロワー情報のURL。

- url

  プロフィールのURL。通常のブラウザで表示できるリンク。

- manuallyApprovesFollowers

  フォローすることを承認制にするか。これはMastodon等で独自に追加されているもの。

なお、以上の形式は```application/activity+json```をContent-Typeのヘッダーに付けて返すようにする。

以上のような形式でActivityPubが成り立っている。その他にもあるが、それについては追々紹介しよう。
