# BTCマイニング簡易収益計算CLI

無料かつ認証不要で利用できる外部APIから BTC の円建て価格を取得し、BTC マイニングの簡易収益を計算する Python 製 CLI ツールです。

## ツール概要

CLI で以下を入力すると、最新の BTC 価格を使って収益を計算します。

- ハッシュレート (TH/s)
- 消費電力 (W)
- 電気代 (円/kWh)
- 想定採掘量 (BTC/日)

出力内容は以下です。

- 取得した BTC 価格（円/BTC）
- 1日の電気代（円）
- 1日の売上（円）
- 1日の利益（円）
- 30日間の利益（円）

## 採用したAPI

- 名称: bitFlyer Lightning API の HTTP Public API
- エンドポイント: `GET https://api.bitflyer.com/v1/ticker?product_code=BTC_JPY`

## このAPIを選んだ理由

- 日本円建ての `BTC_JPY` ティッカーを公式に提供しているため
- Public API と Private API が明確に分かれており、価格取得の用途に合っているため
- 単純な GET リクエストで価格取得でき、標準ライブラリのみで実装しやすいため

## 無料かつ認証不要と判断した根拠の要約

bitFlyer の公式ドキュメントでは HTTP API が Public API と Private API に分かれており、価格取得に使う `/v1/ticker` は Public API として案内されています。認証用ヘッダーが必要なのは Private API のみであり、今回利用する価格取得では不要です。Public API の説明内で `BTC_JPY` を指定した ticker 取得例とレスポンス項目 `ltp` が示されているため、この値を円建て価格として利用しています。

公式ドキュメント:

- https://lightning.bitflyer.com/docs?lang=ja#http-api

## セットアップ方法

`uv` を使ってセットアップします。

```bash
uv sync
```

## 実行方法

```bash
uv run python main.py
```

実行後、案内に従って数値を入力してください。

## 実行例

```text
$ uv run python main.py
BTCマイニング簡易収益計算ツール
以下の値を入力してください。
ハッシュレート (TH/s): 100
消費電力 (W): 3000
電気代 (円/kWh): 31
想定採掘量 (BTC/日): 0.00012

計算結果
BTC価格: 14,500,000円 / BTC
1日の電気代: 2,232円
1日の売上: 1,740円
1日の利益: -492円
30日間の利益: -14,760円
```

※ BTC価格は API の取得時点で変動します。

## ディレクトリ構成

```text
.
├── main.py
├── pyproject.toml
└── README.md
```

## API通信失敗時の挙動

以下の失敗を日本語のエラーメッセージで表示して終了します。

- HTTP エラー
- タイムアウト
- JSON 解析エラー
- 想定外のレスポンス形式
- DNS 解決失敗などの通信エラー

## 実装上の補足

- 標準ライブラリの `urllib` で API 通信を実装しています
- API 差し替えをしやすくするため、価格取得処理は `fetch_btc_jpy_price()` に分離しています
- 入力値はすべて `float` として扱い、0 以下や数値以外はエラーにしています
