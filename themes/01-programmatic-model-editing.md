# 01 プログラムによるSimulinkモデルのブロックとコンフィギュレーションの変更

- 確認日: 2026-10-11
- ドキュメントの版: 日本語版のページはR2026aの表示（日本語版は英語版より更新が遅れることがあり、各ページに英語版へのリンクがある）

## 概要

MATLABのスクリプトから、Simulinkモデルのブロックの追加・置換・削除・パラメーター変更や、コンフィギュレーションパラメーター（ソルバー、コード生成の設定等）の変更を行える。多数のモデルへの同じ変更、設定の統一、モデルの自動生成などに使う。Simulink本体の機能で、追加のツールボックスは要らない。

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

## リンク

### ドキュメント（ブロックの操作）

| リンク | 説明 | 製品・リリース |
|---|---|---|
| [プログラムによるモデル編集](https://jp.mathworks.com/help/simulink/programmatic-modeling.html) | カテゴリのページ。モデルの作成・編集・検索に使う関数と解説の一覧 | Simulink |
| [プログラムによるモデル化の基礎](https://jp.mathworks.com/help/simulink/ug/approach-modeling-programmatically.html) | モデルの作成・読み込み・パラメーター設定・保存の基本の流れ。最初に読む入門 | Simulink |
| [プログラムによるブロックのパラメーターとプロパティの指定](https://jp.mathworks.com/help/simulink/slref/block-parameters-and-properties.html) | `get_param`・`set_param`でブロックのパラメーターや位置・向きを変える例 | Simulink |
| [set_param](https://jp.mathworks.com/help/simulink/slref/set_param.html) | モデル・ブロック・線・端子のパラメーターを設定する。複数のパラメーターは1回の呼び出しでまとめて設定するのが推奨 | Simulink |
| [get_param](https://jp.mathworks.com/help/simulink/slref/get_param.html) | パラメーターの名前と値を取得する。設定できるパラメーター名を調べるのにも使う | Simulink |
| [find_system](https://jp.mathworks.com/help/simulink/slref/find_system.html) | 条件を指定してブロック・線・端子等を検索する。マスクの下やライブラリリンクは既定では検索しない点に注意 | Simulink |
| [Simulink.findBlocks](https://jp.mathworks.com/help/simulink/slref/simulink.findblocks.html) | ブロックを検索してハンドルを返す。`find_system`より新しい検索の関数 | Simulink、R2018a以降 |
| [Simulink.findBlocksOfType](https://jp.mathworks.com/help/simulink/slref/simulink.findblocksoftype.html) | ブロックの種類（BlockType）を指定して検索する | Simulink、R2018a以降 |
| [add_block](https://jp.mathworks.com/help/simulink/slref/add_block.html) | ライブラリ等のブロックをモデルに追加する。追加と同時にパラメーターも指定できる | Simulink |
| [add_line](https://jp.mathworks.com/help/simulink/slref/add_line.html) | ブロック間を線でつなぐ | Simulink |
| [Add, Copy, Replace, and Delete Blocks Programmatically](https://jp.mathworks.com/help/simulink/ug/add-copy-replace-and-delete-blocks-programmatically.html) | ブロックの追加・コピー・置換・削除をまとめた解説（英語） | Simulink |
| [replace_block](https://jp.mathworks.com/help/simulink/slref/replace_block.html) | 条件に合うブロックを別のブロックに一括で置き換える。`"noprompt"`で確認のダイアログを省ける | Simulink |
| [Simulink.BlockDiagram.arrangeSystem](https://jp.mathworks.com/help/simulink/slref/simulink.blockdiagram.arrangesystem.html) | ブロックの配置と線を自動で整える。指定した階層だけが対象 | Simulink、R2018a以降 |
| [Configure Model Layout Programmatically](https://jp.mathworks.com/help/simulink/ug/configure-model-layout-programmatically.html) | ブロックの位置・大きさ・線の配置をプログラムで整える方法（英語） | Simulink |

### ドキュメント（コンフィギュレーション）

| リンク | 説明 | 製品・リリース |
|---|---|---|
| [モデル コンフィギュレーション セット](https://jp.mathworks.com/help/simulink/configuration-sets.html) | カテゴリのページ。コンフィギュレーションセットの関数と解説の一覧 | Simulink |
| [スクリプトを使用したモデル コンフィギュレーションの自動化](https://jp.mathworks.com/help/simulink/ug/configuration-set-api.html) | アクティブなセットの取得・変更、コピーして別のセットを追加・切り替え、2つのセットの差分の比較（`isequal`）までの例 | Simulink |
| [モデルのコンフィギュレーション セットの管理](https://jp.mathworks.com/help/simulink/ug/setting-up-configuration-sets.html) | 1つのモデルに複数のセットを持たせ、切り替える考え方 | Simulink |
| [Simulink.ConfigSet](https://jp.mathworks.com/help/simulink/slref/simulink.configset.html) | コンフィギュレーションセットのオブジェクト | Simulink、R2006a以降 |
| [getActiveConfigSet](https://jp.mathworks.com/help/simulink/slref/getactiveconfigset.html) | モデルのアクティブなセットを取得する | Simulink |
| [attachConfigSet](https://jp.mathworks.com/help/simulink/slref/attachconfigset.html) | セットをモデルに関連付ける（関連付けただけではアクティブにならない） | Simulink |
| [setActiveConfigSet](https://jp.mathworks.com/help/simulink/slref/setactiveconfigset.html) | 関連付けたセットをアクティブにする | Simulink |
| [Simulink.ConfigSetRef](https://jp.mathworks.com/help/simulink/slref/simulink.configsetref.html) | モデルから独立したセットを参照させ、複数のモデルで設定を共有する | Simulink、R2007a以降 |

### ブログ

| リンク | 説明 | 製品・リリース |
|---|---|---|
| [Simulink API で MATLAB から Simulink を操る](https://blogs.mathworks.com/japan-community/2020/02/19/control-simulink-from-matlab-with-simulink-api/) | 日本語。`get_param`・`set_param`の基本と、コンフィギュレーションパラメーターの取得・変更の例（MATLAB ユーザーコミュニティー、2020年） | Simulink |
| [Time to convert from find_system to Simulink.findBlocks](https://blogs.mathworks.com/simulink/2022/04/12/time-to-convert-from-find_system-to-simulink-findblocks/) | 英語。`find_system`から`Simulink.findBlocks`への移行の勧め（Guy on Simulink、2022年） | Simulink、R2018a以降 |
| [Searching in Simulink](https://blogs.mathworks.com/simulink/2008/03/04/searching-in-simulink/) | 英語。`find_system`の既定の動作（マスクの下・ライブラリリンク・線を検索しない等）の注意（Guy on Simulink、2008年。古い記事） | Simulink |
| [How to programmatically configure a Simulink simulation?](https://blogs.mathworks.com/simulink/2025/06/20/how-to-programmatically-configure-a-simulink-simulation/) | 英語。モデルを書き換える`set_param`と、モデルを変えずに条件を変えて実行する`Simulink.SimulationInput`の使い分け（Guy on Simulink、2025年） | Simulink |

### 動画

| リンク | 説明 | 製品・リリース |
|---|---|---|
| [How to Interact with Simulink Models from MATLAB Scripts](https://www.youtube.com/watch?v=pLYZRCY-RO8) | 英語。MATLABのスクリプトでSimulinkモデルを作成・操作する方法（YouTube「MATLAB」チャンネル） | Simulink |

## 補足

- 講演資料・ウェビナー資料: このテーマ（モデル編集のAPI）を主題にした資料は、調査時点の検索では見つからなかった。
- 日本語の動画: 「MATLAB Japan」チャンネルで、このテーマを主題にした動画は見つからなかった。
- `find_system`の`Variants`引数は将来のリリースで削除予定とされている。バリアントを含むモデルの検索では`MatchFilter`を使う（`find_system`のページ参照）。
