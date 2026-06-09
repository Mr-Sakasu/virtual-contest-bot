# Virtual Contest Bot
![Uploading image.png…]()

AtCoder Problems のバーチャルコンテスト一覧を巡回し、開始前・本日開催・終了直後のコンテスト情報を X/Twitter に通知する bot です。

- GitHub: https://github.com/Mr-Sakasu/virtual-contest-bot
- 種別: 競技プログラミング支援 bot、通知自動化、Web scraping
- 対象: AtCoder Problems Virtual Contest

## 作成物の説明

AtCoder Problems の Virtual Contest ページを Selenium で取得し、コンテスト名、開始時刻、終了時刻、URL を抽出します。抽出したコンテストを現在時刻との差分で分類し、通知対象となるイベントだけを X/Twitter API 経由で投稿する構成にしています。

## 担当した役割

- Selenium と BeautifulSoup を使った Virtual Contest ページの取得・解析を実装
- コンテストの開始前、開催中、終了直後、本日開催、明日以降、過去開催を分類する状態管理を実装
- 通知文面の整形と X/Twitter への投稿処理を実装
- Heroku 系の Procfile、Python runtime、headless Chromium 実行設定を整備

## 直面した課題と解決方法

- Virtual Contest ページが JavaScript で描画されるため、HTML を直接取得するだけでは一覧を読めませんでした。Selenium の headless browser で描画後の DOM を取得し、BeautifulSoup で `tr` 要素を解析する構成にしました。
- 同じ行が複数回拾われる場合があるため、取得した HTML を set で重複排除してからコンテスト情報を抽出しています。
- 通知対象を絞らないと投稿数が多くなるため、開始 10 分以内、終了 10 分以内、本日開催のコンテストだけを投稿対象にしています。
- サーバー環境で GUI なしに動かす必要があるため、headless Chromium と `--no-sandbox`、`--disable-dev-shm-usage` などのオプションを指定しています。

## 技術情報

- Python 3.11
- Selenium
- BeautifulSoup4
- Tweepy
- python-dotenv
- Headless Chromium / ChromeDriver
- Procfile による起動コマンド定義

## 環境変数

X/Twitter API の認証情報を `.env` または実行環境の環境変数として設定します。

```bash
TWITTER_API_KEY=...
TWITTER_API_SECRET=...
ACCESS_TOKEN=...
ACCESS_TOKEN_SECRET=...
```

## Commands

```bash
pip install -r requirements.txt
python analyzer.py
```

デプロイ環境では `Procfile` により以下が実行されます。

```bash
python analyzer.py
```

## 関連リポジトリ

- Portfolio: https://github.com/Mr-Sakasu/portfolio
- 競技プログラミング: https://github.com/Mr-Sakasu/abc
- Typing Game: https://github.com/Mr-Sakasu/typing-game
