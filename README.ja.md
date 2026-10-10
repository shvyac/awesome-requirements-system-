# Awesome Requirements Tree

[English](README.md) | 日本語

**全車両**または**全社ITシステム**レベルの最上位要求を**階層ツリー**で管理し、**開発完了後にツリー構造を更新し続ける**（実装後の同期、ベースライン、トレーサビリティ）ためのOSSキュレーションリストです。

## 目次

- [コードとしての要求管理（テキスト + Git）](#コードとしての要求管理テキスト--git)
- [要求エディタ / GUI](#要求エディタ--gui)
- [MBSE: 車両全体・システムモデル](#mbse-車両全体システムモデル)
- [Capella と StrictDoc の連携](#capella-と-strictdoc-の連携)
- [EA: 全社ITシステム](#ea-全社itシステム)
- [トレーサビリティ](#トレーサビリティ)
- [おすすめの組み合わせ](#おすすめの組み合わせ)

## コードとしての要求管理（テキスト + Git）

- [doorstop-dev/doorstop](https://github.com/doorstop-dev/doorstop) ![stars](https://img.shields.io/github/stars/doorstop-dev/doorstop?style=flat-square) - バージョン管理を使った要求管理。文書間の親子リンクでツリーを作る。
- [strictdoc-project/strictdoc](https://github.com/strictdoc-project/strictdoc) ![stars](https://img.shields.io/github/stars/strictdoc-project/strictdoc?style=flat-square) - 技術文書と要求管理。Web UIで階層を編集できる。
- [useblocks/sphinx-needs](https://github.com/useblocks/sphinx-needs) ![stars](https://img.shields.io/github/stars/useblocks/sphinx-needs?style=flat-square) - Sphinx上で要求（needs）を管理。ISO 26262系のプロジェクトで使われる。
- [bmw-software-engineering/trlc](https://github.com/bmw-software-engineering/trlc) ![stars](https://img.shields.io/github/stars/bmw-software-engineering/trlc?style=flat-square) - BMW製の "Treat Requirements Like Code"。構造変更を静的にチェックできる。

## 要求エディタ / GUI

- [sevendays/doorhole](https://github.com/sevendays/doorhole) ![stars](https://img.shields.io/github/stars/sevendays/doorhole?style=flat-square) - Doorstop用のグラフィカルな要求エディタ。
- [shuart/ephemeris](https://github.com/shuart/ephemeris) ![stars](https://img.shields.io/github/stars/shuart/ephemeris?style=flat-square) - システムズエンジニアリングと要求管理のアプリ（JavaScript）。
- [osrmt/osrmt](https://github.com/osrmt/osrmt) ![stars](https://img.shields.io/github/stars/osrmt/osrmt?style=flat-square) - OSSの要求管理ツール（Java）。

## MBSE: 車両全体・システムモデル

- [dbinfrago/py-capellambse](https://github.com/dbinfrago/py-capellambse) ![stars](https://img.shields.io/github/stars/dbinfrago/py-capellambse?style=flat-square) - CapellaモデルをPythonからヘッドレスで操作。開発後の一括更新をスクリプト化できる。
- [Systems-Modeling/SysML-v2-Release](https://github.com/Systems-Modeling/SysML-v2-Release) ![stars](https://img.shields.io/github/stars/Systems-Modeling/SysML-v2-Release?style=flat-square) - SysML v2 仕様のリリース。
- [Systems-Modeling/SysML-v2-API-Services](https://github.com/Systems-Modeling/SysML-v2-API-Services) ![stars](https://img.shields.io/github/stars/Systems-Modeling/SysML-v2-API-Services?style=flat-square) - SysML v2 モデルの参照・更新用REST API。
- [Open-MBEE/exec-cameo-mdk](https://github.com/Open-MBEE/exec-cameo-mdk) ![stars](https://img.shields.io/github/stars/Open-MBEE/exec-cameo-mdk?style=flat-square) - Cameo用プラグイン。モデル同期（MMS）と文書生成。
- [gaphor/gaphor](https://github.com/gaphor/gaphor) ![stars](https://img.shields.io/github/stars/gaphor/gaphor?style=flat-square) - 軽量なSysML/UMLモデリングツール。

## Capella と StrictDoc の連携

要求ツリー（StrictDoc）とシステムアーキテクチャモデル（Eclipse Capella）をつなぐ方法です。

**連携パターン（下記パイロットで示されているもの）**

```text
StrictDoc（.sdoc、正本の要求、Git管理）
   -> ReqIF 1.0 エクスポート（StrictDoc標準機能）
   -> Eclipse Capella + Requirements Viewpoint（ReqIFを手動インポート / Diff-Merge）

要求 <-> アーキテクチャのリンク: 両ツールの外側の trace-links.yaml で管理（MIDとUUIDで対応付け）
Capellaモデルの読み取り: py-capellambse（読み取り専用）
```

- 要求ツリーは `.sdoc` を唯一の正本とし、変更のたびにReqIFを再エクスポートする。
- 交換データをバージョン管理（例: Gitタグ `v1` / `v2`）し、`SPEC-OBJECT/@IDENTIFIER`（UID）でReqIFスナップショットを比較して差分を把握する。
- 要求側からCapellaのファイルへは書き込まず、モデルの読み取りはpy-capellambse経由で行う。

**パイロットリポジトリ（初期段階、執筆時点でstar 0）**

- [dechernov-stack/reqpilot-sc](https://github.com/dechernov-stack/reqpilot-sc) ![stars](https://img.shields.io/github/stars/dechernov-stack/reqpilot-sc?style=flat-square) - StrictDoc 0.29.0 -> ReqIF 1.0 -> Capella 7.1.0（Requirements Viewpoint 0.14.0）の相互運用パイロット。v1/v2のReqIFスナップショットとUIDによる比較を含む。
- [dechernov-stack/reqpilot-workbench](https://github.com/dechernov-stack/reqpilot-workbench) ![stars](https://img.shields.io/github/stars/dechernov-stack/reqpilot-workbench?style=flat-square) - StrictDocとCapellaの上に載るローカルWebワークベンチ。要求のCRUD、アーキテクチャツリー、トレーサビリティグラフ、カバレッジ行列、影響分析。

**パイロットが報告している制限事項**

- 実機のCapellaでのインポートテストは、どちらのパイロットでも**未実施**（ビルド環境にCapellaのGUIとモデルがなく、フィクスチャのみを使用）。
- StrictDoc 0.29.0 のReqIFネイティブ出力で、キリル文字が `\uXXXX` のまま書き出される問題が報告されている。日本語などの非ASCII文字も、インポート前に確認が必要な可能性がある（日本語では未検証）。
- StrictDoc 0.29.0 と capellambse 0.8.0 は互換性のない `python-datauri` を要求するため、ワークベンチは仮想環境を2つに分けている。

## EA: 全社ITシステム

- [finos/waltz](https://github.com/finos/waltz) ![stars](https://img.shields.io/github/stars/finos/waltz?style=flat-square) - 企業情報サービス。アプリ、データ、組織の関係を管理する。
- [vincentmakes/turbo-ea](https://github.com/vincentmakes/turbo-ea) ![stars](https://img.shields.io/github/stars/vincentmakes/turbo-ea?style=flat-square) - エンタープライズアーキテクチャ管理ソリューション。
- [ModelioOpenSource/Modelio](https://github.com/ModelioOpenSource/Modelio) ![stars](https://img.shields.io/github/stars/ModelioOpenSource/Modelio?style=flat-square) - ArchiMate、TOGAF、UML、BPMNに対応するモデリングツール。
- [tractorjuice/arc-kit](https://github.com/tractorjuice/arc-kit) ![stars](https://img.shields.io/github/stars/tractorjuice/arc-kit?style=flat-square) - AIコーディングアシスタントで要求とトレーサビリティ付きのアーキテクチャガバナンスを回す。

## トレーサビリティ

- [itsallcode/openfasttrace](https://github.com/itsallcode/openfasttrace) ![stars](https://img.shields.io/github/stars/itsallcode/openfasttrace?style=flat-square) - 要求トレーシングスイート。
- [elisa-tech/BASIL](https://github.com/elisa-tech/BASIL) ![stars](https://img.shields.io/github/stars/elisa-tech/BASIL?style=flat-square) - 要求のトレーサビリティを持つソフトウェア品質管理ツール。

## おすすめの組み合わせ

| 対象 | 最上位モデル | 要求ツリー | 開発後の更新 |
|---|---|---|---|
| 車両全体 | Capella または SysML v2 | StrictDoc / Doorstop | py-capellambse / SysML v2 API でスクリプト更新。Capella と StrictDoc は ReqIF で連携（上記参照） |
| 全社ITシステム | waltz / turbo-ea | StrictDoc | Gitでベースラインを再同期し、OpenFastTraceで追跡 |

## コントリビュート

プルリクエスト歓迎です。1行に1エントリ、短い説明を付けて追加してください。
