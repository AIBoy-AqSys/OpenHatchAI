# RESPONSIBILITY_BOUNDARY.md

## 境界
このプロジェクトのマニフェストは以下の2系統を含みます。

- BUILD（サーバをどう構築するか）
- RUNTIME_POLICY（サーバ側エージェントがどう動作すべきか）

## 構築側AIの責務
- BUILD / BUILD_VERIFICATION の実施
- RUNTIME_POLICY の「配置・参照可能化」まで
- 実運用Issueの解決は行わない

## サーバ側常駐エージェントの責務
- `main` 最新化 → 作業ブランチ作成
- 自律修正 / 検証ループ
- ブラウザ検証の活用
- PR作成と進捗報告
- 停止条件に基づく報告停止
