# MATLAB/Simulink 公式情報のテーマ別リンク集

MATLAB/Simulinkの公式情報は量が多い一方で散らばっており、検索エンジンから目的のページに一度でたどり着けないことがある。このリンク集は、モデルベース開発（MBD）での利用を想定し、テーマごとに「何ができるか（概要）」と「どこを読めばよいか（リンク）」を1枚にまとめたものである。調査と整理はClaude Codeが行っている。

MathWorks社とは無関係の非公式のリンク集である。MATLAB・Simulinkは The MathWorks, Inc. の登録商標。

## テーマ一覧

| No. | テーマ | ファイル | 確認日 |
|---|---|---|---|
| 01 | プログラムによるSimulinkモデルのブロックとコンフィギュレーションの変更 | [themes/01-programmatic-model-editing.md](themes/01-programmatic-model-editing.md) | 2026-10-11 |
| 02 | 設計情報からのテスト自動生成 | [themes/02-test-generation-from-design.md](themes/02-test-generation-from-design.md) | 2026-10-11 |
| 03 | プラントモデルをニューラルネットワークに置き換えるサロゲート化 | [themes/03-surrogate-model-with-neural-network.md](themes/03-surrogate-model-with-neural-network.md) | 2026-10-11 |

## 情報源の範囲

- MathWorksのドメイン（`mathworks.com`とそのサブドメイン。`jp.mathworks.com`・`blogs.mathworks.com`等）のページ。ドキュメント・製品ページ・ブログ・ビデオ・講演資料・ウェビナー資料を含む。
- YouTubeの公式チャンネル「MATLAB」（`@MATLAB`）と「MATLAB Japan」。
- MATLAB Answers等のコミュニティの回答は、MathWorksの社員以外の回答も含むため、載せる場合はその旨を書き添える。
- 第三者のブログ等は載せない。どの情報源も、文章を転載せず、リンクと自分の言葉での要約だけを載せる。

## 1テーマのファイルの書式

テーマごとに`themes/NN-<英語の短い名前>.md`を1ファイル作る。

1. **見出しと確認日**: テーマ名、調査した日付、調査時点の最新リリース（分かる場合）。
2. **概要**: 何ができるか、主な関数・アプリ・ブロック、必要な製品（ツールボックス）。
3. **読む順番の目安**: 初めて調べる人がどこから読めばよいか。
4. **リンク**: 種類ごとの表に分ける（ドキュメント／ブログ／講演資料・ウェビナー／動画／コミュニティ）。各行に次を書く。
   - リンク（ページの題名）
   - 一言の説明
   - 必要な製品と対応リリース（分かる場合。「R2018a以降」等）
5. **補足**: 注意点（古い情報、名前の変わった機能等）、見つからなかったもの。

## URLの書き方

- 検索結果やページの取得で実在を確かめたURLだけを載せる。推測で組み立てたURLは載せない。
- ドキュメントは`jp.mathworks.com`で載せる（日本語版がない場合も日本語サイト上の英語版が開く）。`jp.`版の実在も確かめる。
- 国別のサブドメイン（`in.`・`se.`・`kr.`等）は使わない。製品ページ等で`jp.`版がないもの、動画のページ（`/videos/`）、講演資料のPDFは`www.mathworks.com`で載せる。
- 検索で見つかっても、`www.`・`jp.`のどちらも404になるページ（移動・統合されたもの）は載せず、テーマの「補足」に書く。
- 末尾の`_ja_JP.html`等の言語別ファイル名や、検索の追跡用のクエリ文字列は取り除く。
- YouTubeは`https://www.youtube.com/watch?v=<ID>`の形にそろえる。

## 調べる手順

1. **ドキュメント**: テーマの関数名・機能名（英語と日本語）で、`mathworks.com`にドメインを絞ってWeb検索する。カテゴリのページ（関数の一覧）と、入門のページ（例題つきの説明）を起点にする。
2. **ブログ**: `blogs.mathworks.com`に絞って検索する（英語の「Guy on Simulink」等、日本語の「MATLAB ユーザーコミュニティー」等）。
3. **講演資料・ウェビナー**: 「MATLAB EXPO」に限らず、テーマのキーワード（日本語・英語）で`mathworks.com`内を検索する。新しい年の資料は検索で見つかりにくいので、MathWorksの「[イベントの講演資料を検索](https://jp.mathworks.com/company/events/conferences/search-proceedings.html)」のページも使う。見つからなければ「補足」にその旨を書く。
4. **動画**: `youtube.com`と`mathworks.com/videos`に絞って検索する。YouTubeの動画は、oEmbed（`https://www.youtube.com/oembed?url=<動画のURL>&format=json`）の`author_name`で公式チャンネル（「MATLAB」「MATLAB Japan」）の投稿かを確かめる。動画の中身は確認できないので、題名と説明文から判断する。
5. **確認**: 載せるURLの実在（`jp.`版を含む）を確かめ、確認日を書く。`mathworks.com`はボット対策のため`curl`等の単純な要求を拒む（HEADは405、GETは403）ので、Claude CodeのWebFetchでページを取得し、題名・言語・導入リリース（「R2018a で導入」等）を読み取って確かめる。
