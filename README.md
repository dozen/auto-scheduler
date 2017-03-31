# Auto Scheduler

これは[waker](https://github.com/ryotarai/waker)のエスカレーションルールをローテーションするためのツールです。

Googleカレンダーの繰り返し機能を使うと日曜日に毎回1番目に立たされる人がでてくるので、それを避けるために使います。

`1->2->3` → `3->1->2` → `2->3->1` という順番でエスカレーションルールを入れ替えながら、Googleカレンダーに予定を作成します。

指定した月のカレンダーに、指定した条件（曜日、休日、平日）で予定を作成します。

## client_id.jsonの用意
[API manager](https://console.cloud.google.com/apis/credentials)で `認証情報を作成→OAuthクライアントID→アプリケーションの種類: その他` と進む。

JSONをダウンロードする( `client_secret_xxxx.json` )。

`client_secret_xxxx.json` を `client_id.json` にリネームする。

## 使用例

```
鈴木、松本、稲葉の順から始まる予定を入れる。
カレンダーの名前はwaker
休日が条件
18:30から、翌日9:30まで
5月を指定
```

```
./auto-scheduler -e "鈴木->松本->稲葉" -n "waker" -c "holiday" -t 18:30-9:30 -d "->" -m 5
```

![Google Calendar](https://raw.githubusercontent.com/dozen/auto-scheduler/master/doc/img/calendar.png)

## Options

option | example val | description | required
------ | ------ | ------ | ----- |
--escalation-series, -e | dean->sam->cas | エスカレーションの順番。 | yes
--name, -n | infra-waker | 予定を入れるカレンダーの名前 | yes
--conditions, -c | monday,holiday | 複数指定可。曜日・平日(weekday)・休日(holiday) | yes
--term, -t | 9:30-18:00 | 開始時刻-終了時刻。終了時刻が開始時刻より早い場合は翌日とみなす | yes
--delimiter, -d | -> | 順番の区切り文字。 wakerの設定と合わせる | yes
--month, -m | 5 | 予定を入れる月を指定 | yes

オプションは全部必須です。

デリミタはなんでも構いませんが、wakerの設定と合わせる必要があります。また、 `-e` オプションと `-d` オプションのデリミタが一致するようにしてください。

