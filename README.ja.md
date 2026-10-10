# Awesome Requirements AI

[English](README.md) | 日本語

**全車両**または**全社ITシステム**レベルの最上位要求を**階層ツリー**で管理し、**開発完了後にツリー構造を更新し続ける**（実装後の同期、ベースライン、トレーサビリティ）ための、そして**AI（LLM、エージェント、MCP）を使ってそれを行う**ためのOSSキュレーションリストです。

## 目次

- [コードとしての要求管理（テキスト + Git）](#コードとしての要求管理テキスト--git)
- [要求エディタ / GUI](#要求エディタ--gui)
- [MBSE: 車両全体・システムモデル](#mbse-車両全体システムモデル)
- [Capella と StrictDoc の連携](#capella-と-strictdoc-の連携)
- [EA: 全社ITシステム](#ea-全社itシステム)
- [トレーサビリティ](#トレーサビリティ)
- [商用ツールとGitHub上の連携リポジトリ](#商用ツールとgithub上の連携リポジトリ)
- [AI × 要求](#ai--要求)
- [関連リスト](#関連リスト)
- [おすすめの組み合わせ](#おすすめの組み合わせ)

## コードとしての要求管理（テキスト + Git）

- [doorstop-dev/doorstop](https://github.com/doorstop-dev/doorstop) ![stars](https://img.shields.io/github/stars/doorstop-dev/doorstop?style=flat-square) - バージョン管理を使った要求管理。文書間の親子リンクでツリーを作る。
- [strictdoc-project/strictdoc](https://github.com/strictdoc-project/strictdoc) ![stars](https://img.shields.io/github/stars/strictdoc-project/strictdoc?style=flat-square) - 技術文書と要求管理。Web UIで階層を編集できる。
- [florath/rmtoo](https://github.com/florath/rmtoo) ![stars](https://img.shields.io/github/stars/florath/rmtoo?style=flat-square) - 無料のOSS要求管理ツール（Python）。要求をプレーンテキストで管理し、依存関係・トレーサビリティのチェックとLaTeX/PDF/HTML出力ができる。
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
- [goeb/reqflow](https://github.com/goeb/reqflow) ![stars](https://img.shields.io/github/stars/goeb/reqflow?style=flat-square) - 要求トレーサビリティツール（C++）。
- [elisa-tech/BASIL](https://github.com/elisa-tech/BASIL) ![stars](https://img.shields.io/github/stars/elisa-tech/BASIL?style=flat-square) - 要求のトレーサビリティを持つソフトウェア品質管理ツール。

## 商用ツールとGitHub上の連携リポジトリ

以下のツールは商用（OSSではない）ため、本体はメインのエントリには載せていません。GitHubで見つかった連携用リポジトリだけを載せています。IBM DOORS Next、ReqView、Cradle は、確認できる公式リポジトリがありませんでした。

- [valispace/ValispacePythonAPI](https://github.com/valispace/ValispacePythonAPI) ![stars](https://img.shields.io/github/stars/valispace/ValispacePythonAPI?style=flat-square) - Valispace（要求・システムズエンジニアリング）のPython API。
- [valispace/ValispaceExcelPlugin](https://github.com/valispace/ValispaceExcelPlugin) ![stars](https://img.shields.io/github/stars/valispace/ValispaceExcelPlugin?style=flat-square) - ValispaceのExcelプラグイン。
- [Kio-pon/innoslate-mcp](https://github.com/Kio-pon/innoslate-mcp) ![stars](https://img.shields.io/github/stars/Kio-pon/innoslate-mcp?style=flat-square) - ログイン済みのブラウザ経由で、AIアシスタントがInnoslateのプロジェクトを読み書きするMCPサーバー（非公式、初期段階）。
- [FrancoisMalan/innoslate-xml-parser](https://github.com/FrancoisMalan/innoslate-xml-parser) ![stars](https://img.shields.io/github/stars/FrancoisMalan/innoslate-xml-parser?style=flat-square) - InnoslateのエクスポートXMLを解析してカスタムCSVにする（古い、2019年）。

## AI × 要求

以下の多くは新しくstarの少ないプロジェクトです。利用前に成熟度を確認してください。説明は各リポジトリのGitHub上の説明文に基づきます。

### エージェントスキルと仕様駆動開発

- [Agile-V/agile_v_skills](https://github.com/Agile-V/agile_v_skills) ![stars](https://img.shields.io/github/stars/Agile-V/agile_v_skills?style=flat-square) - トレーサブルな要求、独立した検証、人による承認ゲート、監査可能なAI支援エンジニアリングのためのエージェントスキル（Claude Code、Cursor、VS Code、GitHub Copilot対応）。
- [sudokar/openspec-plus](https://github.com/sudokar/openspec-plus) ![stars](https://img.shields.io/github/stars/sudokar/openspec-plus?style=flat-square) - OpenSpecの仕様駆動開発を拡張するエージェントスキル。探索、要求、設計判断、実行計画を支援。
- [FredAntB/Spec-Driven-Development](https://github.com/FredAntB/Spec-Driven-Development) ![stars](https://img.shields.io/github/stars/FredAntB/Spec-Driven-Development?style=flat-square) - コードの前に requirements.md、design.md、tasks.md を生成し、複数のAIコーディングツール用の設定ファイルも作るClaudeスキル。
- [agent-ix/quoin](https://github.com/agent-ix/quoin) ![stars](https://img.shields.io/github/stars/agent-ix/quoin?style=flat-square) - Claude Code向けの仕様駆動開発。検証済みのISO/IEC/IEEE 29148準拠の仕様を作成。
- [muchengracedriver/awesome-requirements-writer](https://github.com/muchengracedriver/awesome-requirements-writer) ![stars](https://img.shields.io/github/stars/muchengracedriver/awesome-requirements-writer?style=flat-square) - エンジニアリングの意図を、設計・レビュー・検証・追跡できる要求に変換。不足入力は閾値を作り出さずに指摘する。
- [kj648/requirement-clarifier](https://github.com/kj648/requirement-clarifier) ![stars](https://img.shields.io/github/stars/kj648/requirement-clarifier?style=flat-square) - あいまいな要求を、検証済みでトレース可能な仕様に変えるAIコーディングエージェント向けワークフロー。

### LLMによる分析・トレーサビリティ・変更影響

- [saima-imran/safe-trace-ai](https://github.com/saima-imran/safe-trace-ai) ![stars](https://img.shields.io/github/stars/saima-imran/safe-trace-ai?style=flat-square) - 安全要求の変更影響分析、トレーサビリティ、検証証拠レビューをAIで支援する研究プロトタイプ（Ollama、Streamlit）。
- [rioffe/speccheck](https://github.com/rioffe/speccheck) ![stars](https://img.shields.io/github/stars/rioffe/speccheck?style=flat-square) - SPEC.md が宣言する各IDについて、コードとテストでの参照箇所を探し、証拠付きで1IDにつき1ステータスを報告。オプションで判定を下げる方向だけのLLM判定。
- [ahmedsalem84/Req-RAG-LLM](https://github.com/ahmedsalem84/Req-RAG-LLM) ![stars](https://img.shields.io/github/stars/ahmedsalem84/Req-RAG-LLM?style=flat-square) - RAG強化LLMに知識グラフの構築とトレーサビリティ最適化を組み合わせた要求工学。
- [boxdate/ReqPipeline](https://github.com/boxdate/ReqPipeline) ![stars](https://img.shields.io/github/stars/boxdate/ReqPipeline?style=flat-square) - USDMとローカルLLM（Ollama）による要求工学パイプライン。

### 要求ツール向けMCPサーバー

- [XORwell/jama-cli](https://github.com/XORwell/jama-cli) ![stars](https://img.shields.io/github/stars/XORwell/jama-cli?style=flat-square) - Jama要求管理のCLI。AIアシスタント向けにMCPサーバーにも対応。
- [arthurfantaci/requirements-advisor](https://github.com/arthurfantaci/requirements-advisor) ![stars](https://img.shields.io/github/stars/arthurfantaci/requirements-advisor?style=flat-square) - 要求管理のベストプラクティスを助言するMCPサーバー。
- [aminfara/matrix](https://github.com/aminfara/matrix) ![stars](https://img.shields.io/github/stars/aminfara/matrix?style=flat-square) - マルチエージェント・マルチセッション作業向けの、ローカルでプロジェクト単位の要求管理MCPサーバー。
- [Kio-pon/innoslate-mcp](https://github.com/Kio-pon/innoslate-mcp) ![stars](https://img.shields.io/github/stars/Kio-pon/innoslate-mcp?style=flat-square) - Innoslate向けMCPサーバー（上の商用ツールのセクション参照）。

### LLMエージェントによるMBSE

- [cosmindxu/mbse-workflow](https://github.com/cosmindxu/mbse-workflow) ![stars](https://img.shields.io/github/stars/cosmindxu/mbse-workflow?style=flat-square) - 階層化されたSysML風モデルを作成し、各ステップをSysproseのチェックで関門にするLLMエージェントのワークフロー。

### AIシステム自体の要求

- [responsibleai/ASSERT](https://github.com/responsibleai/ASSERT) ![stars](https://img.shields.io/github/stars/responsibleai/ASSERT?style=flat-square) - AIエージェントとLLMアプリ向けの要求駆動の評価ハーネス。要求から振る舞い別のテストケースを生成。
- [ybeven/4D-ARE](https://github.com/ybeven/4D-ARE) ![stars](https://img.shields.io/github/stars/ybeven/4D-ARE?style=flat-square) - LLMエージェント向けの、帰属（attribution）駆動の要求工学フレームワーク。

### AI支援による更新ループの案（ツールではなくワークフローの提案）

1. 開発後に、変更点（差分、実装後のモデル、テスト結果）を集める。
2. LLMが要求ツリーとトレースリンクの変更案を、変更影響分析つきで提案する。
3. 人がレビューして承認する。ツリーはGit管理（`.sdoc` / Doorstop）にして、変更をレビュー可能な差分にする。
4. ReqIFを再エクスポートし、トレーサビリティを再チェックする。

## 関連リスト

- [jgsystemsconsulting/awesome-requirements-engineering](https://github.com/jgsystemsconsulting/awesome-requirements-engineering) - 要求工学のリソース集（標準、手法、交換フォーマット、ツール、書籍、コミュニティ）。
- [ishandutta2007/Awesome-Engineering-Requirements-Management](https://github.com/ishandutta2007/Awesome-Engineering-Requirements-Management) - OSSの工学向け要求管理ツール集。

## おすすめの組み合わせ

| 対象 | 最上位モデル | 要求ツリー | 開発後の更新 |
|---|---|---|---|
| 車両全体 | Capella または SysML v2 | StrictDoc / Doorstop | py-capellambse / SysML v2 API でスクリプト更新。Capella と StrictDoc は ReqIF で連携（上記参照） |
| 全社ITシステム | waltz / turbo-ea | StrictDoc | Gitでベースラインを再同期し、OpenFastTraceで追跡 |
| どちらもAI併用 | 上記と同じ | 上記と同じ | LLMがツリー変更と影響を提案し、人がGitで承認（AIセクション参照） |

## コントリビュート

プルリクエスト歓迎です。1行に1エントリ、短い説明を付けて追加してください。
