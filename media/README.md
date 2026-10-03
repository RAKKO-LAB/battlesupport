# media — 紹介動画（PV）

| ファイル | 内容 |
|---|---|
| pv_ja.mp4 / pv_ja.webm | 16.5秒PV・日本語版（1280×720・無音）。トップのヒーローで再生 |
| pv_ja_poster.jpg | 上記のポスター（0.8秒時点） |
| pv_en.mp4 / pv_en.webm / pv_en_poster.jpg | 英語版（`pv/en.html`）。英語トップ `/en/` のヒーローで再生。アプリ画面は英語表示で撮り直した `pv/assets/en_*.mp4` / `en_matchup.webp` |

正本はコンポジション `pv/index.html`（HyperFrames）。作り直す手順:

```bash
cd pv
npx --yes hyperframes@0.8.111 check
npx --yes hyperframes@0.8.111 render --video-frame-format png -o renders/pv_ja_master.mp4
npx --yes hyperframes@0.8.111 render -c en.html --video-frame-format png -o renders/pv_en_master.mp4   # 英語版（ffmpeg 3行は ja→en に読み替え）
cd ..
ffmpeg -y -i pv/renders/pv_ja_master.mp4 -vf scale=1280:-2 -an -c:v libx264 -crf 26 -preset slow -pix_fmt yuv420p -movflags +faststart media/pv_ja.mp4
ffmpeg -y -i pv/renders/pv_ja_master.mp4 -vf scale=1280:-2 -an -c:v libvpx-vp9 -b:v 0 -crf 38 -row-mt 1 media/pv_ja.webm
ffmpeg -y -ss 0.8 -i pv/renders/pv_ja_master.mp4 -frames:v 1 -vf scale=1280:-2 -q:v 3 media/pv_ja_poster.jpg
```

- フォントは `pv/assets/fonts/NotoSansJP-sub.ttf`（`index.html` と `en.html` の文字だけに絞ったサブセット。両方の文字を入れて作る）。`check` は index.html しか見ないので、英語版は render 後のフレームで目視する。文言を変えたら
  元フォント（Google Fonts の Noto Sans JP 可変 TTF、gitignore 済み）から `python -m fontTools.subset` で作り直す
- 実機映像 `pv/assets/real_overlay.mp4`（重ねて表示）・`real_read.mp4`（読み取り）・`real_calc.mp4`（ダメ計）は
  `battlesupport-research/Advertisement/video1.mp4` を正立化・30fps 固定にして 4.6s/15.9s/27.0s から切り出し、
  右端のナビゲーションバーを落とした 1536×720（`crop=1536:720:0:0`）。`real_game.webp` は同 0.5s、`real_matchup.webp` は `S5.jpg`
- 冒頭の「別のダメ計アプリ」は特定アプリの画面を使わず HTML で描いた汎用の縦画面（スプライトはアプリの `0445`/`0373_mega`）
- 旧 `overlay_pop.mp4`（エミュレータ・ダミー背景）は未使用
- `check`/`snapshot` が「Navigation timeout of 10000 ms」で落ちる環境では `check --timeout=60000`、render は `--browser-timeout 120`

- 英語版の映像 `pv/assets/en_overlay.mp4`（バブル→パネル）・`en_read.mp4`（撮影→読み取り、4倍速＋手直し後の結果1.2秒）・`en_calc.mp4` は
  エミュレータ録画を `crop=2086:978:0:62,scale=1536:720`（ステータスバーとナビを落として日本語版と同じ 1536×720）にしたもの
