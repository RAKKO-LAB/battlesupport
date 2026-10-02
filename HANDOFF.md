# HANDOFF — サイト刷新・PV制作（2026-10-02 開始）

## 目的
トップページ（`index.html`）を使い方ガイド（`guide/`）並みの品質へ。15秒PVを HyperFrames で作り、トップに埋め込む。
トーンは「対戦の補助・記録ツール」。煽る・挑発的なコピーは使わない。

## フェーズ（各フェーズでコミット＝いつ止めても壊れない）
| # | 内容 | 状態 |
|---|---|---|
| 0 | 現状把握・HyperFrames スキル導入・Sonnet サブエージェント | 完了 |
| 1 | 素材撮影（エミュレータ+adb）→ `assets/` | 完了 |
| 2 | トップ刷新（動画/Playボタン/機能カード/8言語/プライバシー/非公式注記/OGP） | 完了 |
| 3 | 15秒PV（HyperFrames、日本語版）→ `media/`、トップに埋め込み | 完了（英語版は未着手。手順は `media/README.md`） |
| 4 | （余力）ガイドの演出強化 | 未 |

## 素材の所在
- ガイド録画: `guide/media/c1_permission` `c2_launch` `c3_overlay`（重ねて表示の実画面・1.2MB） `c4_fix` `c5_calc` `c6_record`（.webm）、ポスター `poster_overlay.jpg`
- ガイド静止画: `guide/media/s1_consent.jpg`, `s2`〜`s8.jpg`（s5=6×6対面一覧、s6=初手おすすめ）
- アプリ APK: `../battlesupport/app/build/outputs/apk/debug/app-debug.apk`（AVD `Medium_Phone` API 36.1）

## 制約
- アプリ本体（battlesupport リポジトリ）のコードは変更しない
- 新規撮影ではゲームの公式画面を使わない（選出画面が要るなら自作ダミー）。既存ガイド素材は流用可
- 既存 URL・canonical・meta を壊さない。依存なし・1ページ完結・スマホ優先・ライト/ダーク

## 道具
- HyperFrames: `~/.claude/skills/hyperframes*`（`npx hyperframes skills update` で導入済み、CLI は `npx hyperframes`）
- Sonnet 作業者: `~/.claude/agents/sonnet-worker.md`（model: sonnet。次セッションから `subagent_type: sonnet-worker`。
  導入セッションでは general-purpose + model: sonnet で代用し、claude-sonnet-5-5 で動くことを確認済み）
