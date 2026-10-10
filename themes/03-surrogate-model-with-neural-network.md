# 03 プラントモデルをニューラルネットワークに置き換えるサロゲート化

- 確認日: 2026-10-11
- 調査時点の最新リリース: R2026b
- 日本語版のページの表示: 確認していない（日本語版は英語版より更新が遅れることがあり、各ページに英語版へのリンクがある）

## 概要

計算の重いプラントモデル（物理モデル、FEM等の高忠実度モデル）を、その入出力データで学習したニューラルネットワーク等の軽いモデルに置き換える。MathWorksは「低次元化モデリング（Reduced Order Modeling、ROM）」と呼んでいる。シミュレーションの高速化、HILテスト、制御設計、仮想センサー等に使う。精度は元のモデルより落ちるので、許容できる誤差かを確かめて使う。

基本の流れは次の4段階。

1. 実験計画を立て、高忠実度モデルをシミュレーションして入出力データを作る
2. データでROMを学習する
3. 元のモデルと比べて精度を確かめる
4. ROMをSimulinkのブロックとして組み込み、元のサブシステムと置き換える

| 方法 | 主な関数・アプリ・ブロック | 主な製品 |
|---|---|---|
| アプリで一通り行う | Reduced Order Modelerアプリ（`reducedOrderModeler`）。LSTM・非線形ARX・ニューラル状態空間モデル等を選べる | Reduced Order Modeler for MATLAB（アドオン、R2025b以降） |
| ニューラル状態空間モデル | `idNeuralStateSpace`（R2022b以降）、`nlssest`、Neural State-Space Modelブロック | System Identification Toolbox、Deep Learning Toolbox（ネットワークに`dlnetwork`を使う） |
| LSTM等の深層学習ネットワーク | Stateful Predictブロック、`exportNetworkToSimulink`（R2024b以降） | Deep Learning Toolbox |
| 非線形ARX（ガウス過程等） | Nonlinear ARX Modelブロック | System Identification Toolbox、Statistics and Machine Learning Toolbox |

## 読む順番の目安

1. 「低次元化モデリングとは」と、Simulinkのカテゴリのページ「低次元化モデリング」で全体像と手段をつかむ
2. 「Reduced Order Modeling Overview」で、データ駆動の方法（LSTM・ニューラル状態空間等）と線形化に基づく方法の違いを知る
3. 使う手段の例題を読む（アプリなら「Reduced Order Model of a Jet Engine Turbine Blade」、LSTMなら「Simulink における LSTM ネットワークを使用した物理システムのモデリング」、ニューラル状態空間ならEVバッテリーの例題）

## リンク

「用途」の列は、学ぶ順番に沿った使い方（概要／チュートリアル／サンプル／辞書）を示す。分類の基準はREADMEの「[用途の分類](../README.md#用途の分類)」を参照。

### ドキュメント（全体像）

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [低次元化モデリングとは](https://jp.mathworks.com/discovery/reduced-order-modeling.html) | 概要 | 用語の解説ページ。ROMの目的と手法の概要 | — |
| [低次元化モデリング（Simulink）](https://jp.mathworks.com/help/simulink/reduced-order-modeling.html) | 辞書 | Simulinkのカテゴリのページ。ROMの作り方・アプリ・例題の一覧。Reduced Order Modelerアプリのアドオンの入手方法もここ | Simulink ほか |
| [低次元化モデリング（Deep Learning Toolbox）](https://jp.mathworks.com/help/deeplearning/reduced-order-modeling.html) | 辞書 | Deep Learning Toolboxのカテゴリのページ。深層学習によるROMの例題の一覧 | Deep Learning Toolbox |
| [Reduced Order Modeling Overview](https://jp.mathworks.com/help/ident/ug/reduced-order-modeling.html) | 概要 | ROMの手法の分類（データ駆動の静的・動的モデル、線形化に基づく方法）と選び方（英語） | System Identification Toolbox ほか |

### ドキュメント（作り方・例題）

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [Reduced Order Modeler](https://jp.mathworks.com/help/simulink/slref/reducedordermodeler-app.html) | 辞書 | アプリのリファレンス。実験計画→データ生成→学習→Simulinkへのエクスポートを一通り行える（英語） | Reduced Order Modeler for MATLAB、R2025b以降 |
| [Reduced Order Model of a Jet Engine Turbine Blade](https://jp.mathworks.com/help/ident/ug/reduced-order-model-of-a-jet-engine-turbine-blade.html) | サンプル | 例題。タービンブレードの高忠実度モデルからAIベースのROMを作る（英語） | System Identification Toolbox、Deep Learning Toolbox、Partial Differential Equation Toolbox、Reduced Order Modeler for MATLAB |
| [Simulink における LSTM ネットワークを使用した物理システムのモデリング](https://jp.mathworks.com/help/deeplearning/ug/physical-system-modeling-using-lstm-network.html) | サンプル | 例題。物理モデルのデータでLSTMを学習し、Stateful Predictブロックで物理コンポーネントと置き換える | Simulink、Simscape、Deep Learning Toolbox |
| [Reduced Order Modeling of Electric Vehicle Battery System Using Neural State-Space Model](https://jp.mathworks.com/help/ident/ug/reduced-order-modeling-of-electric-vehicle-battery-system-using-neural-state-space-model.html) | サンプル | 例題。EVバッテリーの電圧・温度をニューラル状態空間モデルで予測し、Simulinkのブロックとして使う（英語） | System Identification Toolbox、Deep Learning Toolbox、Simscape Battery、Simscape Fluids |
| [Surrogate Modeling Using Gaussian Process-Based NLARX Model](https://jp.mathworks.com/help/ident/ug/surrogate-modeling-using-gaussian-process-nlarx-model.html) | サンプル | 例題。ニューラルネットワークでなくガウス過程の非線形ARXでサロゲートを作る別の方法（英語） | System Identification Toolbox、Statistics and Machine Learning Toolbox、Simscape |
| [ニューラル状態空間モデル](https://jp.mathworks.com/help/ident/neural-state-space-models.html) | 辞書 | カテゴリのページ。ニューラル状態空間モデルの関数・ブロック・例題の一覧 | System Identification Toolbox |
| [idNeuralStateSpace](https://jp.mathworks.com/help/ident/ref/idneuralstatespace.html) | 辞書 | ニューラル状態空間モデルのオブジェクト。状態と出力の関数はDeep Learning Toolboxの`dlnetwork`で表し、学習は`nlssest`で行う（英語） | System Identification Toolbox、Deep Learning Toolbox、R2022b以降 |

### ドキュメント（Simulinkへの組み込み）

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [Simulink を使用した深層学習](https://jp.mathworks.com/help/deeplearning/deep-learning-with-simulink.html) | 辞書 | カテゴリのページ。学習済みネットワークをSimulinkで使うブロックと例題の一覧 | Deep Learning Toolbox |
| [Simulink でのネットワークの状態の予測と更新](https://jp.mathworks.com/help/deeplearning/ug/predict-update-network-simulink.html) | サンプル | 例題。LSTM等の再帰型ネットワークを、時刻をまたいで状態を保つStateful Predictブロックで動かす | Deep Learning Toolbox、Simulink |
| [exportNetworkToSimulink](https://jp.mathworks.com/help/deeplearning/ref/dlnetwork.exportnetworktosimulink.html) | 辞書 | 学習済みネットワークを、層ごとのブロックで構成したSimulinkモデルに書き出す。入出力が1つずつのネットワークに限る（英語） | Deep Learning Toolbox、R2024b以降 |

### 製品・ソリューションのページ

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [低次元化モデリング（製品のページ）](https://jp.mathworks.com/products/reduced-order-modeling.html) | 概要 | ROMに関する製品・機能の紹介 | — |
| [AI を活用したモデルベースデザイン](https://jp.mathworks.com/solutions/ai-model-based-design.html) | 概要 | ROM・AIサロゲートモデル・仮想センサー等の活用の紹介と、企業の事例 | — |

### ブログ

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [Reduced Order Modeler App: 低次元化モデル（ROM）をインタラクティブに構築](https://blogs.mathworks.com/japan-community/2024/11/08/reduced-order-modeler-app-how-to-interactively-create-ai-based-reduced-order-models-roms-jp/) | チュートリアル | 日本語。アプリでタービンブレードのROMを作り、Simulinkへエクスポートする例（MATLAB ユーザーコミュニティー、2024年。英語の記事の翻訳）。当時はベータ版のサポートパッケージで、現在はReduced Order Modeler for MATLABに置き換わっている | Reduced Order Modeler for MATLAB |
| [Deep Learning Based Surrogate Models](https://blogs.mathworks.com/deep-learning/2021/05/21/deep-learning-based-surrogate-models/) | サンプル | 英語。永久磁石同期モーターの電気・熱の挙動をLSTMのサロゲートで置き換え、Simulinkへ書き出す例（2021年） | Deep Learning Toolbox |

### 動画

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [モデルベース開発の新時代へ - AI 活用が切り開く新境地と効率化](https://www.mathworks.com/videos/ai-with-model-based-design-1722926819909.html) | 概要 | 日本語のウェビナー、約45分（2024年）。ROMによるシミュレーションの高速化、サロゲートモデル、仮想センサー等、AIモデルをSimulinkで使う方法のまとめ | Deep Learning Toolbox ほか |
| [AI with Model-Based Design: Reduced Order Modeling](https://www.mathworks.com/videos/ai-with-model-based-design-reduced-order-modeling-1670274257624.html) | 概要 | 英語、約45分（2024年）。ジェットエンジンのブレードの高忠実度シミュレーションからAIベースのROMを作り、制御設計・HILテスト・組み込みに使う | Deep Learning Toolbox、System Identification Toolbox ほか |
| [What Is the Reduced Order Modeler App?](https://www.mathworks.com/videos/what-is-the-reduced-order-modeler-app-1712575809152.html) | 概要 | 英語、約4分（2026年）。アプリでSimulinkのサブシステムのROMを作る方法の紹介 | Reduced Order Modeler for MATLAB |
| [AI・学習モデルのSimulink実装とコード自動生成](https://www.youtube.com/watch?v=yknhTjZhuoQ) | チュートリアル | 日本語。学習済みのネットワークをSimulinkに組み込み、コードを生成する方法（YouTube「MATLAB Japan」チャンネル） | Deep Learning Toolbox ほか |

## 補足

- Reduced Order Modelerアプリは、以前はFile Exchangeのベータ版のサポートパッケージ（Reduced Order Modeling Support Package）として配布されていた。アプリのリファレンスのページの表示は「R2025bで導入」で、アドオン（Reduced Order Modeler for MATLAB）はアドオン エクスプローラーから入手する。一方、File Exchangeの案内では、ベータ版がReduced Order Modeler for MATLABに置き換わったのはR2025b Update 3以降とされている。R2025bの初期の版で使う場合は、どちらで入手するかを確かめる。古い記事の手順は、名前や入手方法が変わっていることがある。
- 講演資料（PDF）: このテーマを主題にした資料は、調査時点の検索では見つからなかった。MathWorksの「[イベントの講演資料を検索](https://jp.mathworks.com/company/events/conferences/search-proceedings.html)」のページで「低次元化」「サロゲート」等の語で探すと見つかる可能性がある。
