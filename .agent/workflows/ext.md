---
description: 外部サーバ上での作業を行うためのワークフロー
---

1. `.agent/workflows/initialize.md` の手順に従い、ログファイルを作成・開始する。
2. `.env` から `SERVER_HOST`、`SSH_PORT`、`SERVER_USER`、`SSH_KEY_SERVER` の接続情報を取得する。
3. SSH 接続を行う前に、ユーザーへ接続コマンドと接続先を提示し、承認を得る。
4. 承認後、以下の形式で SSH 接続を実行する:
   ```bash
   ssh -i {SSH_KEY_SERVER} -p {SSH_PORT} {SERVER_USER}@{SERVER_HOST}
   ```
5. サーバ上では `/opt/agentd/` 以下、`config.json`、Nginx 設定、systemd ユニットなど、外部サーバ用マニフェストに基づく操作のみを行う。
6. ローカルプロジェクトファイルは変更せず、必要に応じてリモートからの出力や変更結果をログに記録する。
7. 作業完了後は SSH セッションを切断し、ログに「セッション終了」と記載する。