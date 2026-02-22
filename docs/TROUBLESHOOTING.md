# TROUBLESHOOTING.md

## よくある詰まりどころ
- `.env` に APIキーが未投入
- GitHub Webhook 設定権限が不足
- gh CLI 認証は通るが webhook 受信が未整備
- ブラウザ検証基盤は導入済みだが agentd から呼べない
- `12_SERVER_AI_INSTRUCTIONS.md` の配置場所が不明 / 参照パス未設定
- BUILD と RUNTIME_POLICY を混同して、構築段階で実運用Issueを解こうとしてしまう

## 切り分け順
1. 認証（SSH / gh / GitHub権限）
2. `.env` の必須キー有無
3. Webhook受信経路
4. agentd 常駐化
5. ブラウザ検証基盤の利用可能性
6. `12_SERVER_AI_INSTRUCTIONS.md` 参照性
