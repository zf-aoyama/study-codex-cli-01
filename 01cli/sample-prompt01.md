あなたはPythonエンジニアです。
現在のディレクトリ（cliディレクトリ）の中だけで作業してください。
親ディレクトリや他のディレクトリには一切触れないでください。

目的
BTCマイニングの簡易収益を計算するCLIツールをPythonで作成する。

開発条件

* Pythonプロジェクトは **uv** で管理する
* `pyproject.toml` を作成する
* `uv sync` で依存関係をインストールできること
* `uv run python main.py` で実行できること
* 追加ライブラリは基本的に使用しない（標準ライブラリのみ）

作成するファイル

* main.py
* pyproject.toml
* README.md

ツール仕様

ユーザーが以下の値を入力できるCLIツールを作る。

入力

* ハッシュレート (TH/s)
* 消費電力 (W)
* 電気代 (円/kWh)
* BTC価格 (円/BTC)
* 想定採掘量 (BTC/日)

出力

* 1日の電気代（円）
* 1日の売上（円）
* 1日の利益（円）
* 30日間の利益（円）

計算式

1日の電気代
(消費電力 / 1000) * 24 * 電気代

1日の売上
想定採掘量 * BTC価格

1日の利益
1日の売上 - 1日の電気代

30日間の利益
1日の利益 * 30

実装要件

* CLIでユーザー入力を受け取る
* 入力値はfloatとして扱う
* 0以下の値が入力された場合はエラーメッセージを表示する
* 計算処理は関数に分離する
* 出力は日本語で見やすく表示する
* 金額は桁区切りで表示する

main.pyの要件

以下の関数を作る

calculate_daily_electric_cost
calculate_daily_revenue
calculate_daily_profit
calculate_monthly_profit

main()関数から処理を呼び出す構成にする

README.mdの内容

READMEには以下を書く

* ツールの概要
* uvを使ったセットアップ方法
* 実行方法
* 実行例

READMEの実行例

uv sync
uv run python main.py
