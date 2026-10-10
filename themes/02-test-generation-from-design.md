# 02 設計情報からのテスト自動生成

- 確認日: 2026-10-11
- 調査時点の最新リリース: R2026b
- 日本語版のページの表示: R2026bのものがある（ページにより英語版より古いことがあり、各ページに英語版へのリンクがある）

## 概要

設計情報（要件・仕様、モデルの構造）から、テストケースやテスト入力を自動で作る方法は、大きく次の3つに分かれる。

| 方法 | 何から作るか | 主な製品・関数 |
|---|---|---|
| 要件ベースのテスト生成 | Requirements Tableブロックで形式化した要件（仕様モデル） | Requirements Toolbox＋Simulink Design Verifier |
| カバレッジを満たすテスト生成 | 設計モデルそのもの（判定・条件・MC/DC等のカバレッジ目標） | Simulink Design Verifier（`sldvrun`、`sldvoptions`） |
| テストの枠組みの自動作成 | モデルの構成（サブシステム・Model・Stateflowチャート等のコンポーネント） | Simulink Test（Create Test for Componentウィザード、`sltest.testmanager.createTestForComponent`） |

生成したテストは、Simulink TestのTest Managerへエクスポートして実行・管理する（`sltest.import.sldvData`。Requirements Tableから作ったテストは対応する要件に自動でリンクされる）。カバレッジの測定にはSimulink Coverageを使う。いずれも有償の製品が前提で、テーマ全体ではSimulink Test・Simulink Design Verifier・Requirements Toolbox・Simulink Coverageが関わる。

テスト生成の前に、Requirements Tableの要件が完全で矛盾がないかを解析しておくことが推奨されている（不完全だとテスト目標を満たすテストを作れないことがある）。

## 読む順番の目安

1. 「Workflow for Test Generation」で、Simulink Design Verifierによるテスト生成の全体の流れをつかむ
2. 要件から作るなら「Requirements Table ブロックを使用した仕様モデルの作成」→「Requirements Table ブロックからのテストの生成とエクスポート」→例題「要件に基づくテストに対する仕様モデルの使用」
3. カバレッジの穴を埋めるなら「テスト生成用のモデル カバレッジ オブジェクティブ」と「既存テスト ケースの定義および拡張」
4. Test Managerでの管理は「SimulinkTest へのテスト ケースのエクスポート」と`sltest.import.sldvData`

## リンク

「用途」の列は、学ぶ順番に沿った使い方（概要／チュートリアル／サンプル／辞書）を示す。分類の基準はREADMEの「[用途の分類](../README.md#用途の分類)」を参照。

### ドキュメント（Simulink Design Verifierによるテスト生成）

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [体系的モデル検証における Simulink Design Verifier の使用](https://jp.mathworks.com/help/sldv/gs/systematic-model-verification-using-simulink-design-verifier.html) | 概要 | 入門。モデルの体系的な検証に製品をどう使うかの全体像 | Simulink Design Verifier |
| [Workflow for Test Generation](https://jp.mathworks.com/help/sldv/ug/workflow-test-case-generation.html) | チュートリアル | テスト生成の手順の全体像（英語） | Simulink Design Verifier |
| [テスト生成用のモデル カバレッジ オブジェクティブ](https://jp.mathworks.com/help/sldv/ug/model-coverage-objectives-for-test-generation.html) | 辞書 | 判定・条件・MC/DC等、どのカバレッジを目標にテストを作るかの設定 | Simulink Design Verifier |
| [既存テスト ケースの定義および拡張](https://jp.mathworks.com/help/sldv/ug/defining-and-extending-existing-tests-cases.html) | チュートリアル | 既存のテストケースを起点に、タイムステップを追加して拡張し、テストスイートを効率よく作る | Simulink Design Verifier |
| [SimulinkTest へのテスト ケースのエクスポート](https://jp.mathworks.com/help/sldv/ug/create-test-cases-from-simulink-design-verifier-results.html) | チュートリアル | 生成したテストをSimulink TestのTest Managerへ取り込む | Simulink Design Verifier、Simulink Test |

### ドキュメント（要件からのテスト生成）

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [Requirements Table ブロックを使用した仕様モデルの作成](https://jp.mathworks.com/help/sldv/ug/what-is-specification-model.html) | チュートリアル | 文章の要件をRequirements Tableで形式化し、テスト生成に使う「仕様モデル」を作る | Requirements Toolbox、Simulink Design Verifier |
| [Requirements Table ブロックからのテストの生成とエクスポート](https://jp.mathworks.com/help/sldv/ug/export-generated-tests-from-requirements-table.html) | チュートリアル | 仕様モデルからテストを生成し、Test Managerへエクスポートする手順。スクリプト（`sldvoptions`・`sldvrun`）での実行例もある | Requirements Toolbox、Simulink Design Verifier、Simulink Test |
| [要件に基づくテストに対する仕様モデルの使用](https://jp.mathworks.com/help/sldv/ug/use-specification-model-for-requirements-based-testing.html) | サンプル | 例題。仕様モデルで生成したテストを設計モデルに対して実行し、カバレッジを確かめる | Simulink、Simulink Coverage、Simulink Design Verifier、Requirements Toolbox |
| [機能テストの実行とテスト カバレッジの解析](https://jp.mathworks.com/help/slrequirements/ug/functional-testing-and-coverage-analysis.html) | チュートリアル | 要件ベースのテストを実行し、カバレッジの不足を確かめて対処する流れ | Requirements Toolbox、Simulink Test、Simulink Coverage、Simulink Design Verifier |

### ドキュメント（Simulink Testでのテストの枠組みの作成）

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [Generate Tests and Test Harnesses for a Model or Components](https://jp.mathworks.com/help/sltest/ug/generate-tests-and-test-harnesses-for-a-model-or-components.html) | チュートリアル | モデルやコンポーネントごとに、テストハーネスとテストケースをウィザードで作る（英語） | Simulink Test |
| [sltest.testmanager.createTestForComponent](https://jp.mathworks.com/help/sltest/ref/sltest.testmanager.createtestforcomponent.html) | 辞書 | 上のウィザードと同じことをスクリプトで行う。複数のコンポーネントにまとめて作れる（英語） | Simulink Test、R2020b以降 |
| [sltest.import.sldvData](https://jp.mathworks.com/help/sltest/ref/sltest.import.sldvdata.html) | 辞書 | Simulink Design Verifierの解析結果から、テストハーネスとテストファイルを作る | Simulink Test、Simulink Design Verifier、R2015b以降 |

### 製品・ソリューションのページ

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [Simulink Design Verifier 製品情報](https://jp.mathworks.com/products/simulink-design-verifier.html) | 概要 | カバレッジ目標やカスタム目標のテストケースを生成し、要件ベースのテストを補う | Simulink Design Verifier |
| [Simulink Test 製品情報](https://jp.mathworks.com/products/simulink-test.html) | 概要 | シミュレーションによるテストの作成・管理・実行、要件とのトレース | Simulink Test |
| [Requirements Toolbox 製品情報](https://jp.mathworks.com/products/requirements-toolbox.html) | 概要 | 要件の作成・リンク・妥当性確認。Requirements Tableブロックで形式的な要件を書ける | Requirements Toolbox |

### ブログ

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [Simulink Agentic Toolkit](https://blogs.mathworks.com/simulink/2026/04/17/simulink-agentic-toolkit/) | サンプル | 英語。AIエージェントにテスト計画を立てさせ、コンポーネント単位と閉ループのテストをTest Managerに実装させた例（Guy on Simulink、2026年） | Simulink Agentic Toolkit、Simulink Test |

### 講演資料（PDF）

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [モデルに対するテスト・検証プロセスの効率化（PDF）](https://www.mathworks.com/content/dam/mathworks/mathworks-dot-com/solutions/automotive/files/jp-expo-2014/model-verification-and-validation-solution.pdf) | 概要 | 日本語。MATLAB EXPO 2014の講演資料。Simulink Design Verifierによるテストケースの自動生成と、スクリプトによる検証作業の自動化（古い資料） | Simulink Design Verifier ほか |

### 動画

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [How to Generate Requirements-Based Test Cases](https://www.mathworks.com/videos/requirements-based-testing-89778.html) | チュートリアル | 英語、約4分（2023年）。Requirements Tableの仕様モデルからSimulink Design Verifierでテストを生成する | Requirements Toolbox、Simulink Design Verifier |
| [Formalizing Requirements and Generating Requirements Based Test Cases](https://www.mathworks.com/videos/formalizing-requirements-and-generating-requirements-based-test-cases-1683216502105.html) | チュートリアル | 英語、約21分（2023年）。文章の要件をRequirements Tableで形式化し、抜け・矛盾を確かめてからテストを自動生成する | Requirements Toolbox、Simulink Design Verifier |
| [Requirements-Based Verification with Simulink Test](https://www.mathworks.com/videos/requirements-based-verification-with-simulink-test-1554822878413.html) | 概要 | 英語、約22分（2019年）。文章の要件を、時間の条件を含む実行可能な判定（temporal assessment）に置き換えてテストする | Simulink Test |

## 補足

- 文章の要件から直接テストを生成する専用の機能（AIによるもの等）は、調査時点の公式ドキュメントでは見つからなかった。要件をRequirements Tableで形式化してから生成するのが公式の流れ。AIエージェントを使う方法は、上のブログ（Simulink Agentic Toolkit）が近い。
- 検索で見つかった次の英語のページは、調査時点で`www.`・`jp.`ともに404だった（ページの移動・統合の可能性がある）ため載せていない: Simulink Testの「Increase Coverage by Generating Test Inputs」、「Automatically Create a Set of Test Cases」。
- 日本語の動画: 「MATLAB Japan」チャンネルで、テストの自動生成を主題にした動画は見つからなかった。
- 新しい年の講演資料（MATLAB EXPO 2025・2026等）は、検索では見つからなかった。
