# 00_OVERVIEW.md（全体目的と流れ）
責務種別: `BUILD + RUNTIME_POLICY`

## 何を行うのか
- このマニフェスト群の目的と全体像を定義する
- **BUILD（サーバ構築）** と **RUNTIME_POLICY（サーバ側エージェント運営方針）** の境界を明示する
- 構築側エージェントが実行すべき順序と、構築後に常駐エージェントが行うことを分離して扱えるようにする
- Webゲーム開発基盤と、Issue駆動の外部サーバAIエージェント実行基盤を一体として設計する

## このマニフェスト群の位置づけ（混乱防止）
- 本マニフェスト群は **「サーバをどう構築するか」** と **「サーバ側エージェントがどう動作すべきか」** の複合設計である
- 構築側エージェントはまず `BUILD` / `BUILD_VERIFICATION` の章を優先して実施する
- `RUNTIME_POLICY` は、構築後にサーバ側で動く常駐エージェントが守るべき規律として導入・配置・有効化する
- `BUILD + RUNTIME_POLICY` の章は、**構築時の導入項目** と **常駐後の運用ルール** を分けて読む

## 責務種別の見方
- `BUILD`：構築側エージェントが実施する
- `BUILD_VERIFICATION`：構築完了後の導入確認として実施する
- `RUNTIME_POLICY`：サーバ側常駐エージェントの運用ルール
- `BUILD + RUNTIME_POLICY`：導入と運用方針の両方を含む

## 全体目的
- VPS上で常駐AIエージェント（agentd）を動作させる
- GitHub Issueコメントをトリガにジョブ化し、Issue解決に向けた自律修正/検証ループを回す
- `main` 最新化 → 作業ブランチ作成 → 修正/検証反復 → PR作成までを一貫運用する
- GitHubゲート（保護ブランチ/CI）を安全装置として使う
- ブラウザ検証を導入し、UI系不具合や再現確認に積極活用する
- AI Provider（`codex` / `claude` / `gemini`）を切替可能にする
- Web→PWA→必要に応じてストア展開へ繋がるゲーム開発基盤を用意する

## 構築側エージェントの最短実行順（BUILD優先）
1. `01_SERVER_OS_GUIDE.md` でOS方針を確定する
2. `02_SERVER_BOOTSTRAP.md` でVPS初期構成を行う
3. `.env` を ` .env_sample `（拡張版）に基づいて配置・確認する
4. `03_AGENTD_DAEMON.md` の BUILD 節に沿って agentd 基盤を導入する
5. `06_GITHUB_GATES.md` / `08_WORKFLOWS_AS_MD.md` でGitHubゲートとCI方針を反映する
6. `09_SECURITY_MINIMUM.md` で最低限セキュリティを確認する
7. `12_SERVER_AI_INSTRUCTIONS.md` をサーバ側の基盤指示書として配置する
8. `11_CHECKLIST.md` で導入確認（BUILD_VERIFICATION）を行う

## 構築後に常駐エージェントが行うこと（RUNTIME_POLICY）
- Issueコメントの起動指示を解釈してジョブを実行する
- 実行前に `main` を最新化し、作業ブランチを作成する
- 自律修正/検証ループ（コード検証 + ブラウザ検証）を回す
- 条件達成時にPRを作成し、進捗/完了/エラーをIssueへ通知する
- 停止条件に達した場合は到達点・未解決点・次の打ち手を整理して報告する
