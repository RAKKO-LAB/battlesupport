# media — 紹介動画（PV）

| ファイル | 内容 |
|---|---|
| pv_ja.mp4 / pv_ja.webm | 15秒PV・日本語版（1280×720・無音）。トップのヒーローで再生 |
| pv_ja_poster.jpg | 上記のポスター（0.8秒時点） |

正本はコンポジション `pv/index.html`（HyperFrames）。作り直す手順:

```bash
cd pv
npx --yes hyperframes@0.8.111 check
npx --yes hyperframes@0.8.111 render --video-frame-format png -o renders/pv_ja_master.mp4
cd ..
ffmpeg -y -i pv/renders/pv_ja_master.mp4 -vf scale=1280:-2 -an -c:v libx264 -crf 26 -preset slow -pix_fmt yuv420p -movflags +faststart media/pv_ja.mp4
ffmpeg -y -i pv/renders/pv_ja_master.mp4 -vf scale=1280:-2 -an -c:v libvpx-vp9 -b:v 0 -crf 38 -row-mt 1 media/pv_ja.webm
ffmpeg -y -ss 0.8 -i pv/renders/pv_ja_master.mp4 -frames:v 1 -vf scale=1280:-2 -q:v 3 media/pv_ja_poster.jpg
```

- フォントは `pv/assets/fonts/NotoSansJP-sub.ttf`（PV の文字だけに絞ったサブセット）。文言を変えたら
  元フォント（Google Fonts の Noto Sans JP 可変 TTF、gitignore 済み）から `python -m fontTools.subset` で作り直す
- 重ねて表示の映像 `pv/assets/overlay_pop.mp4` はエミュレータ録画（背景は自作ダミー画面）。可変fpsのままだと
  フレームが拾われないので 30fps 固定で再エンコードしてある
