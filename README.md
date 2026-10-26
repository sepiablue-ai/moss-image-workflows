# MOSS Image ComfyUI ワークフロー集 (MOSS Image Workflows)

🌐 **日本語** · [English](README_EN.md)

**オープンソースの大規模画像生成基盤モデル「MOSS-Image」（INT8 ConvRot + 8-step DMD蒸留LoRA）を活用し、漫画ページ・日本式アニメ絵コンテ用紙・シネマティックイメージボード・マルチパネル構図など、実践的なクリエイティブ制作に直結するComfyUIワークフロー集です。**

> **Author**: かみもと (Kamimoto) | 速報・検証結果: **[X (@sep_is_heim)](https://x.com/sep_is_heim)** | 解説記事: **[note (かみもと｜ローカルAI生成)](https://note.com/sepiablue)**

---

## 概要

本リポジトリは、オープンソースの大規模画像生成基盤モデル **MOSS-Image** を用いた、実践的・実用的なComfyUIワークフローを集約・公開するコレクション置き場です。

漫画制作（ダイナミックなコマ割り・キャラクター掛け合い・フキダシ・日本語セリフ・オノマトペ配置）、商業アニメーションの絵コンテ用紙、映画・プリビズ用のイメージボードグリッドなど、MOSS-Imageの強力なテキスト描画能力や精密なネイティブJSONレイアウト制御、8ステップ高速推論を活かした多彩なワークフローを順次拡充しています。

- **8ステップ超高速生成**: `moss_image_base_8step_dmd.safetensors` により、わずか8ステップで破綻のない美麗な構図と高解像度出力を実現。
- **MOSSネイティブJSONレイアウト制御**: バウンディングボックス（bbox）指定により、漫画の斜め変形コマ割り・フキダシ・描き文字、絵コンテ表の罫線や日本語印刷テキストと枠内描画を1パスで精密制御。
- **マルチパネル制御（ComfyUI-MOSS-Storyboard連携）**: 2列×3行の6コマグリッドにおいて、コマごとの個別プロンプト指定（`[PANEL 1]`〜`[PANEL 6]`）と厳密な罫線描画・個別トリミングを実現。
- **オリジナル画風LoRA対応**: 美しい光と影、透明感あるシネマティックアニメ調を表現する `SepiaBlue_Luminous_MOSS-Image_v1`（トリガー: `qsv2style`）を採用。

---

## 収録ワークフロー一覧

| ファイル名 | カテゴリ | 解像度 / 構成 | 主な特徴・用途 |
|---|---|---|---|
| [`page_kakeai.json`](page_kakeai.json) | モノクロ漫画ページ / 掛け合い | 1024×1536 (7コマ変形割り) | 漫画家と編集者の締切掛け合い。斜めコマ割り・縦書き/横書きフキダシ・描き文字・キャラ固定bbox制御を1枚完結 |
| [`ekonte_p1.json`](ekonte_p1.json) | 日本式アニメ絵コンテ | 1216×1440 (縦長用紙) | 絵コンテ「空の郵便」1/2。表組み・日本語印刷文字・鉛筆スケッチの一括生成 |
| [`ekonte_p2.json`](ekonte_p2.json) | 日本式アニメ絵コンテ | 1216×1440 (縦長用紙) | 絵コンテ「空の郵便」2/2。後半カット（第4〜6カット）の絵コンテ用紙 |
| [`imageboard_cafe.json`](imageboard_cafe.json) | ポリッシュド・イメージボード | 6コマ (2×3, 各コマ 4:3) | カフェの日常情景。日本語明朝体キャプション枠付き、光の美しいアニメ調 |
| [`imageboard_rainy_cat.json`](imageboard_rainy_cat.json) | ポリッシュド・イメージボード | 6コマ (2×3, 各コマ 4:3) | 雨の日の少女と猫。情緒的な光と色彩のストーリーボード |
| [`imageboard_sky_post.json`](imageboard_sky_post.json) | ポリッシュド・イメージボード | 6コマ (2×3, 各コマ 4:3) | 飛行艇と島々を巡る空の郵便配達。冒険の息吹を感じるシネマティックボード |
| [`sketch_grid.json`](sketch_grid.json) | ラフスケッチ・グリッド | 6コマ (2×3, 各コマ 16:9) | セピアモノクロ鉛筆スケッチ。余計な文字を排除した純粋な画面構成・演出検討用 |

---

## 各ワークフローの詳細

### 1. モノクロ漫画ページ・掛け合いネーム (Manga Page & Dialogue Layout)

本格的な青年漫画スタイルのモノクロ原稿・ネームを、高密度なハッチングやスクリーントーン、ダイナミックな変形コマ割り、フキダシ内の正確な日本語セリフ、手描き風オノマトペ（描き文字）とともに1枚の画像として生成します。

- **`page_kakeai.json`**: 漫画家ミオと担当編集者黒澤の締切当日の掛け合い（全7コマ）
- **生成解像度**: 1024 × 1536 (縦長原稿)
- **レイアウト構成**: 均等なグリッドや単なる横段ではなく、斜めの太い白 gutter（コマ間余白）で分割されたモダンな変形7コマ割り。
- **ネイティブJSON制御**: MOSS-Image ネイティブの JSON プロンプト形式（`summary` + `items` による `abs_centralized` バウンディングボックス制御）を活用。
  - **キャラクター配置 & 整合性固定**: 銀髪ボブ・パーカー姿の漫画家「ミオ」と、黒髪ローポニーテール・眼鏡・スーツ姿の編集者「黒澤」の2名のみを定義し、各コマのバウンディングボックス内で役割とポーズを指定。
  - **日本語テキスト・描き文字描画**: コマ1のナレーション枠（「締切当日、」「午前4時。」）、コマ2のドアを蹴破る巨大描き文字（「バンッ」）やセリフ（「原稿は？」）、コマ3〜6の掛け合い（「…ここにはいない」「居留守は無駄。」「白紙も芸術です」「三時間で描け。」「鬼ーっ！」）、コマ7の静かなオチ（「コーヒーは」「淹れてあげる。」）まで、縦書き（tategaki）・横書きを厳密に座標指定。
- **サンプラー設定**: Euler / `sgm_uniform` / 8 steps / CFG 1.0

### 2. 日本式アニメ絵コンテ用紙 (Classic E-Konte Sheets)

日本の商業アニメーション制作で伝統的に用いられる絵コンテ（e-konte）用紙を、白紙の上に罫線表・印刷フォント・各カットの鉛筆スケッチごと1枚の画像として生成します。

- **`ekonte_p1.json`**: 絵コンテ「空の郵便」 1／2（前半3カット）
- **`ekonte_p2.json`**: 絵コンテ「空の郵便」 2／2（後半3カット）
- **生成解像度**: 1216 × 1440
- **仕組み**: MOSS-Image ネイティブの JSON プロンプト形式（`summary` + `items` による `abs_centralized` バウンディングボックス制御）を活用。ヘッダー（カット／画面／内容／セリフ／秒数）と各行のテキストセル、ピクチャー枠の正確な配置を実現しています。
- **サンプラー設定**: Euler / `sgm_uniform` / 8 steps / CFG 1.0

### 3. ポリッシュド・アニメイメージボード (Polished Anime Imageboard Grid)

映画やアニメーションのプリビズ・美術設定・シーン展開を一覧できる、完成度の高いイメージボードグリッドです。

- **`imageboard_cafe.json`** (カフェ)
- **`imageboard_rainy_cat.json`** (雨の日の猫)
- **`imageboard_sky_post.json`** (空の郵便)
- **レイアウト**: 2列 × 3行（計6コマ）、各コマ 4:3 比率。
- **スタイルLoRA**: `SepiaBlue_Luminous_MOSS-Image_v1.safetensors` (強度 1.0, トリガー: `qsv2style`)
- **カスタムノード**: `ComfyUI-MOSS-Storyboard` (`MossStoryboardLayout`, `MossStoryboardSampler`)
- **特徴**: 各イラストの下に繊細な日本語明朝体（Mincho serif）のキャプションとカット番号枠がレイアウトされ、視線誘導とストーリー展開がひと目で把握できます。

### 4. ラフスケッチ・絵コンテグリッド (Loose Sketch Storyboard Grid)

余計な装飾や文字を取り払い、純粋な構図・ポージング・カメラアングル・明暗の設計に特化したプリプロダクション用スケッチボードです。

- **`sketch_grid.json`**
- **レイアウト**: 2列 × 3行（計6コマ）、各コマ 16:9 シネマティック画角。
- **テイスト**: クリーム色の紙に描かれたセピア調グラファイト鉛筆スケッチ（Rough graphite pencil sketch）。
- **プロンプト指定**: `MossStoryboardSampler` の `[STYLE] ... [PANEL 1] ... [PANEL 6]` 記法を使用。飛行艇のパイロット少女と老灯台守の出会いと別れを映画的なカメラワークで描き出します。

---

## 必要なモデルと配置先

各ワークフローを読み込む前に、以下のモデルファイルを所定のフォルダに配置してください。

| 種類 | モデルファイル名 | 配置先ディレクトリ | 備考 |
|---|---|---|---|
| **UNet / Diffusion** | `moss_image_base_int8_convrot.safetensors` | `ComfyUI/models/unet/` (または `diffusion_models/`) | MOSS-Image INT8 ConvRot ベースモデル |
| **CLIP / Text Encoder** | `qwen3.5_9b_int8_convrot.safetensors` | `ComfyUI/models/clip/` | Type: `moss_image` |
| **VAE** | `flux2-vae.safetensors` | `ComfyUI/models/vae/` | FLUX.2 VAE |
| **Speed LoRA** | `moss_image_base_8step_dmd.safetensors` | `ComfyUI/models/loras/` | 8-step DMD蒸留LoRA (強度 1.0) |
| **Style LoRA** | `SepiaBlue_Luminous_MOSS-Image_v1.safetensors` | `ComfyUI/models/loras/moss/` | `imageboard_*.json` で使用 (トリガー: `qsv2style`) |

---

## 必要なカスタムノード

- **[ComfyUI-MOSS-Storyboard](https://github.com/sepiablue-ai/ComfyUI-MOSS-Storyboard)**
  - 提供ノード: `MossStoryboardLayout`, `MossStoryboardSampler`, `MossStoryboardCrop`
  - インストール方法: `ComfyUI/custom_nodes/` に本リポジトリを git clone するか、ComfyUI Manager から導入してください。
- **ComfyUI 本体**: PR #17095 以降の MOSS-Image ネイティブ対応版（ComfyUI ≥ 0.42.0 推奨）。

---

## 著者情報・関連リンク

**かみもと (Kamimoto)**  
ローカル環境（RTX 4070 12GB等）での動画・画像生成AIモデルの高速化・省VRAM化・ワークフロー設計を検証・公開しています。

- **X (Twitter)**: [@sep_is_heim](https://x.com/sep_is_heim) — ステップ数・秒数・画質の実測速報
- **note**: [かみもと｜ローカルAI生成](https://note.com/sepiablue) — 詳細な解説記事・ノウハウまとめ
- **GitHub**: [@sepiablue-ai](https://github.com/sepiablue-ai)

### 関連リポジトリ
- [ComfyUI-MOSS-Storyboard](https://github.com/sepiablue-ai/ComfyUI-MOSS-Storyboard): MOSS-Image用ストーリーボード・宮格生成カスタムノード
- [minimax_h3_workflows](https://github.com/sepiablue-ai/minimax_h3_workflows): MiniMax H3 動画生成ワークフロー集
- [ComfyUI-H3-X2-Stream](https://github.com/sepiablue-ai/ComfyUI-H3-X2-Stream): INT8 X2 VAE & 非同期NVENC保存ノード

---

## ライセンス (License)

本リポジトリのワークフローファイルは [MIT License](LICENSE) のもとで公開されています。個人利用・商用利用・改変・再配布を問わず自由にご利用いただけます。

※ワークフローが参照する各種モデルチェックポイント、LoRA、カスタムノード等の権利および利用規約は、それぞれの提供元・ライセンサーの規定に従ってください。
