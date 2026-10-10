# 01 プログラムによるSimulinkモデルのブロックとコンフィギュレーションの変更

- 確認日: 2026-10-11
- 調査時点の最新リリース: R2026b
- 日本語版のページの表示: R2026a（各ページに英語版へのリンクがある）

## 概要

MATLABのスクリプトから、Simulinkモデルのブロックの追加・置換・削除・パラメーター変更や、コンフィギュレーションパラメーター（ソルバー、コード生成の設定等）の変更を行える。多数のモデルへの同じ変更、設定の統一、モデルの自動生成などに使う。モデル編集のAPI自体はSimulink本体の機能である（ただし、コード生成など他の製品に属するパラメーターを設定するには、その製品が要る）。

| やりたいこと | 主な関数 |
|---|---|
| モデルの作成・読み込み・保存 | `new_system`、`load_system`、`open_system`、`save_system` |
| ブロック・モデルのパラメーターの取得・変更 | `get_param`、`set_param` |
| ブロックの検索 | `find_system`、`Simulink.findBlocks`、`Simulink.findBlocksOfType`（後の2つはR2018a以降） |
| ブロック・線の追加、置換 | `add_block`、`add_line`、`replace_block`、`delete_block` |
| レイアウトの自動整理 | `Simulink.BlockDiagram.arrangeSystem`（R2018a以降） |
| コンフィギュレーションセットの操作 | `getActiveConfigSet`、`attachConfigSet`、`setActiveConfigSet`、`Simulink.ConfigSet`、`Simulink.ConfigSetRef` |

コンフィギュレーションパラメーターも`get_param`・`set_param`で読み書きできる（モデル名を対象にしても、`getActiveConfigSet`で得たオブジェクトを対象にしてもよい）。複数のモデルで設定を共有するには、独立型のコンフィギュレーションセットを`Simulink.ConfigSetRef`で参照させる。

## 読む順番の目安

1. 「プログラムによるモデル化の基礎」で、モデルの作成から保存までの流れをつかむ
2. ブロックを変えるなら「プログラムによるブロックのパラメーターとプロパティの指定」と`set_param`、設定を変えるなら「スクリプトを使用したモデル コンフィギュレーションの自動化」
3. 必要に応じて各関数のリファレンスと、カテゴリのページ「プログラムによるモデル編集」で関数を探す
4. `set_param`に渡すパラメーターの名前は、ブロックなら各ブロックのリファレンス（「ブロック ライブラリ」から探す）、コンフィギュレーションなら「モデルに対するモデル コンフィギュレーション パラメーターの設定」からたどるペインごとの一覧で調べる

## リンク

「用途」の列は、学ぶ順番に沿った使い方（概要／チュートリアル／サンプル／辞書）を示す。分類の基準はREADMEの「[用途の分類](../README.md#用途の分類)」を参照。

### ドキュメント（ブロックの操作）

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [プログラムによるモデル編集](https://jp.mathworks.com/help/simulink/programmatic-modeling.html) | 辞書 | カテゴリのページ。モデルの作成・編集・検索に使う関数と解説の一覧 | Simulink |
| [プログラムによるモデル化の基礎](https://jp.mathworks.com/help/simulink/ug/approach-modeling-programmatically.html) | チュートリアル | モデルの作成・読み込み・パラメーター設定・保存の基本の流れ。最初に読む入門 | Simulink |
| [プログラムによるブロックのパラメーターとプロパティの指定](https://jp.mathworks.com/help/simulink/slref/block-parameters-and-properties.html) | チュートリアル | `get_param`・`set_param`でブロックのパラメーターや位置・向きを変える例。全ブロック共通のプロパティ（`Position`・`Orientation`等）の一覧表と、ブロック固有のパラメーター名の調べ方（プロパティ インスペクターの「スクリプト リファレンス」、`get_param`の`DialogParameters`等）もここ | Simulink |
| [ブロック ライブラリ](https://jp.mathworks.com/help/simulink/block-libraries.html) | 辞書 | ライブラリ（Sources・Math Operations等）ごとのブロックのカテゴリの一覧。各ブロックのリファレンスのページで、パラメーターごとの「プログラムでの使用」に`set_param`で使う名前と値が書いてある | Simulink |
| [Gain](https://jp.mathworks.com/help/simulink/slref/gain.html) | 辞書 | ブロックのリファレンスの例。「プログラムでの使用」に`Gain`・`SampleTime`・`OutDataTypeStr`等の名前がある（この節がないパラメーターもある） | Simulink |
| [set_param](https://jp.mathworks.com/help/simulink/slref/set_param.html) | 辞書 | モデル・ブロック・線・端子のパラメーターを設定する。複数のパラメーターは1回の呼び出しでまとめて設定するのが推奨 | Simulink |
| [get_param](https://jp.mathworks.com/help/simulink/slref/get_param.html) | 辞書 | パラメーターの名前と値を取得する。設定できるパラメーター名を調べるのにも使う | Simulink |
| [find_system](https://jp.mathworks.com/help/simulink/slref/find_system.html) | 辞書 | 条件を指定してブロック・線・端子等を検索する。マスクの下やライブラリリンクは既定では検索しない点に注意 | Simulink |
| [Simulink.findBlocks](https://jp.mathworks.com/help/simulink/slref/simulink.findblocks.html) | 辞書 | ブロックを検索してハンドルを返す。`find_system`より新しい検索の関数 | Simulink、R2018a以降 |
| [Simulink.findBlocksOfType](https://jp.mathworks.com/help/simulink/slref/simulink.findblocksoftype.html) | 辞書 | ブロックの種類（BlockType）を指定して検索する | Simulink、R2018a以降 |
| [add_block](https://jp.mathworks.com/help/simulink/slref/add_block.html) | 辞書 | ライブラリ等のブロックをモデルに追加する。追加と同時にパラメーターも指定できる | Simulink |
| [add_line](https://jp.mathworks.com/help/simulink/slref/add_line.html) | 辞書 | ブロック間を線でつなぐ | Simulink |
| [delete_block](https://jp.mathworks.com/help/simulink/slref/delete_block.html) | 辞書 | ブロックを削除する。パスかハンドルで指定し、複数をまとめて削除できる | Simulink |
| [Add, Copy, Replace, and Delete Blocks Programmatically](https://jp.mathworks.com/help/simulink/ug/add-copy-replace-and-delete-blocks-programmatically.html) | チュートリアル | ブロックの追加・コピー・置換・削除をまとめた解説（英語） | Simulink |
| [replace_block](https://jp.mathworks.com/help/simulink/slref/replace_block.html) | 辞書 | 条件に合うブロックを別のブロックに一括で置き換える。`"noprompt"`で確認のダイアログを省ける | Simulink |
| [Simulink.BlockDiagram.arrangeSystem](https://jp.mathworks.com/help/simulink/slref/simulink.blockdiagram.arrangesystem.html) | 辞書 | ブロックの配置と線を自動で整える。指定した階層だけが対象 | Simulink、R2018a以降 |
| [Configure Model Layout Programmatically](https://jp.mathworks.com/help/simulink/ug/configure-model-layout-programmatically.html) | チュートリアル | ブロックの位置・大きさ・線の配置をプログラムで整える方法（英語） | Simulink |

### ドキュメント（コンフィギュレーション）

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [モデル コンフィギュレーション セット](https://jp.mathworks.com/help/simulink/configuration-sets.html) | 辞書 | カテゴリのページ。コンフィギュレーションセットの関数と解説の一覧 | Simulink |
| [スクリプトを使用したモデル コンフィギュレーションの自動化](https://jp.mathworks.com/help/simulink/ug/configuration-set-api.html) | チュートリアル | アクティブなセットの取得・変更、コピーして別のセットを追加・切り替え、2つのセットの差分の比較（`isequal`）までの例 | Simulink |
| [モデルのコンフィギュレーション セットの管理](https://jp.mathworks.com/help/simulink/ug/setting-up-configuration-sets.html) | 概要 | 1つのモデルに複数のセットを持たせ、切り替える考え方 | Simulink |
| [モデルに対するモデル コンフィギュレーション パラメーターの設定](https://jp.mathworks.com/help/simulink/ug/configuration-parameters-dialog-box-overview.html) | 辞書 | コンフィギュレーションパラメーターの一覧への入口。ペイン（ソルバー、データのインポート/エクスポート、診断等と、コード生成・Design Verifier等の製品のペイン）ごとの一覧ページにリンクがある。ダイアログでパラメーターを右クリックして「これはなに?」を選ぶと、スクリプトで使う名前が分かる | Simulink ほか |
| [[ソルバー] ペイン](https://jp.mathworks.com/help/simulink/gui/solver-pane.html) | 辞書 | ペインごとの一覧の例。各パラメーターのページの「プログラムでの使用」に`set_param`で使う名前がある（例: ソルバーは`SolverName`） | Simulink |
| [モデル コンフィギュレーション パラメーター: コード生成](https://jp.mathworks.com/help/rtw/ref/code-generation-pane-general.html) | 辞書 | コード生成のペインのパラメーターの一覧（システム ターゲット ファイル、言語等）。最適化・レポート・インターフェイス等の関連ペインにもリンクがある | Simulink Coder |
| [Simulink.ConfigSet](https://jp.mathworks.com/help/simulink/slref/simulink.configset.html) | 辞書 | コンフィギュレーションセットのオブジェクト | Simulink、R2006a以降 |
| [getActiveConfigSet](https://jp.mathworks.com/help/simulink/slref/getactiveconfigset.html) | 辞書 | モデルのアクティブなセットを取得する | Simulink |
| [attachConfigSet](https://jp.mathworks.com/help/simulink/slref/attachconfigset.html) | 辞書 | セットをモデルに関連付ける（関連付けただけではアクティブにならない） | Simulink |
| [setActiveConfigSet](https://jp.mathworks.com/help/simulink/slref/setactiveconfigset.html) | 辞書 | 関連付けたセットをアクティブにする | Simulink |
| [Simulink.ConfigSetRef](https://jp.mathworks.com/help/simulink/slref/simulink.configsetref.html) | 辞書 | モデルから独立したセットを参照させ、複数のモデルで設定を共有する | Simulink、R2007a以降 |

### ブログ

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [Simulink API で MATLAB から Simulink を操る](https://blogs.mathworks.com/japan-community/2020/02/19/control-simulink-from-matlab-with-simulink-api/) | チュートリアル | 日本語。`get_param`・`set_param`の基本と、コンフィギュレーションパラメーターの取得・変更の例（MATLAB ユーザーコミュニティー、2020年） | Simulink |
| [Time to convert from find_system to Simulink.findBlocks](https://blogs.mathworks.com/simulink/2022/04/12/time-to-convert-from-find_system-to-simulink-findblocks/) | 概要 | 英語。`find_system`から`Simulink.findBlocks`への移行の勧め（Guy on Simulink、2022年） | Simulink、R2018a以降 |
| [Searching in Simulink](https://blogs.mathworks.com/simulink/2008/03/04/searching-in-simulink/) | 辞書 | 英語。`find_system`の既定の動作（マスクの下・ライブラリリンク・線を検索しない等）の注意（Guy on Simulink、2008年。古い記事） | Simulink |
| [How to programmatically configure a Simulink simulation?](https://blogs.mathworks.com/simulink/2025/06/20/how-to-programmatically-configure-a-simulink-simulation/) | 概要 | 英語。モデルを書き換える`set_param`と、モデルを変えずに条件を変えて実行する`Simulink.SimulationInput`の使い分け（Guy on Simulink、2025年） | Simulink |

### 動画

| リンク | 用途 | 説明 | 製品・リリース |
|---|---|---|---|
| [How to Interact with Simulink Models from MATLAB Scripts](https://www.youtube.com/watch?v=pLYZRCY-RO8) | チュートリアル | 英語。MATLABのスクリプトでSimulinkモデルを作成・操作する方法（YouTube「MATLAB」チャンネル） | Simulink |

## 補足

- 講演資料・ウェビナー資料: このテーマ（モデル編集のAPI）を主題にした資料は、調査時点の検索では見つからなかった。
- 日本語の動画: 「MATLAB Japan」チャンネルで、このテーマを主題にした動画は見つからなかった。
- 全ブロック共通のプロパティとブロック固有のパラメーターの一覧は、以前は別のページ（`common-block-parameters.html`・`block-specific-parameters.html`）にあったが、調査時点では`www.`・`jp.`ともに404だった。共通のプロパティは「プログラムによるブロックのパラメーターとプロパティの指定」に、ブロック固有のパラメーターは各ブロックのリファレンスに移っている。
- 「プログラムによる内部のモデル設定」（`model-parameters.html`）というページもあるが、MathWorksの内部用に予約されたモデルパラメーターの一覧で、ユーザーが設定するものではないため載せていない。
- `find_system`の`Variants`引数は将来のリリースで削除予定とされている。バリアントを含むモデルの検索では`MatchFilter`を使う（`find_system`のページ参照）。
