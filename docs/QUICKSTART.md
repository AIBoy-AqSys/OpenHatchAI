# QUICKSTART.md
## 目的
OpenHatchAIv1 の構築側エージェントを使って、サーバ側常駐エージェント（agentd）が稼働できる基盤を作る。

## 最短手順
1. `README.md` を読む（全体像と人間作業の把握）
2. `.env_sample` と `.env_sample.extended` を元に `.env` を作成する
3. 必要なキー（GitHub/Webhook/APIキー）を準備する
4. `BUILD_AGENT_PROMPT.md` を構築側AIへ渡す
5. 構築側AIに `manifests/00〜12` を参照させて BUILD / BUILD_VERIFICATION を実行させる
6. `manifests/11_CHECKLIST.md` の観点で導入確認する
7. 未投入の秘密情報を `.env` に入れて、小さいIssueで試験運用する

## 注意
- 構築側AIは BUILD のみ実行する（Issue解決の実運用は行わない）
- `manifests/12_SERVER_AI_INSTRUCTIONS.md` はサーバ側常駐エージェントの基盤指示書としてサーバへ配置する
