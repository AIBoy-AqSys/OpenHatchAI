# 08_WORKFLOWS_AS_MD.md（CIワークフロー方針）
責務種別: `BUILD + RUNTIME_POLICY`

## 何を行うのか
- GitHub Actions のCIワークフロー方針を定義する
- PR / `main` push 時に最低限の品質確認（依存導入・lint・build）を実行する
- 重い処理をVPSではなくCI側に寄せる方針を明確にする
- 将来のテスト追加に備え、CI拡張しやすい構成にする

## 構築時に行うこと（BUILD）
- 最低限CIの導入対象（lint/build）を定義する
- 実行トリガと失敗時の扱いを定義する
- 将来のテスト追加を見越した拡張余地を確保する

## 常駐後の前提（RUNTIME_POLICY）
- agentd はCIを最終品質判定の一部として扱う
- ローカル検証で通っても、CI結果を尊重して再反復の判断を行う
