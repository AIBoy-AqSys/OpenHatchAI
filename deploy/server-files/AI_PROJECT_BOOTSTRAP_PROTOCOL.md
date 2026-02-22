# AI Project Bootstrap Protocol (Standard)

本ドキュメントは、VPS（ターゲットサーバ）上に AI エージェント（agentd）管理下の新プロジェクトを構築するための標準プロトコル（標準手順）を定義します。

> [!NOTE]
> これはあくまで **「標準（Standard）」** であり、憲法ではありません。プロジェクトの特性、技術スタックの進化、またはセキュリティ要件の変化に応じて、必要であれば本ドキュメントを書き換え、より適した手順に最適化することを推奨します。

---

## 1. 事前確認（Pre-flight Checks）

プロジェクト構築を開始する前に、サーバ側の環境を確認します。

- [ ] **Node.js / npm の確認**: 
    - `node -v` (v18+推奨) および `npm -v` が実行可能か。
    - 入っていない場合は `sudo apt-get install -y npm` 等で導入する。
-- [ ] **SSH 鍵の配置**: 
    - `agent` ユーザ配下に GitHub 接続用の SSH 鍵（環境変数 `SSH_KEY_GITHUB` で指定された鍵）が配置されているか。
    - `~/.ssh/config` が設定され、`ssh -T git@github.com` で認証が通るか。
- [ ] **権限確認**: 
    - `/opt/agentd/work/` 配下への書き込み権限があるか（通常は `agent` ユーザが所有）。

## 2. GitHub リポジトリの準備

- **命名規則**: `game01_roguelike` のように、役割が明確な小文字・スネークケースを推奨。
- **リポジトリ作成**: 
    - **AIBoyアカウントの `gh` CLI**（SSH認証済み）で private リポジトリを作成する。
    - ```bash
      gh repo create AIBoy/[プロジェクト名] --private --description "[説明]"
      ```
    - 作成時は `Add a README file` はチェックせず、空のリポジトリとして作成するのがスムーズ。

## 3. サーバー側ブートストラップ（初期化）

サーバ上の `/opt/agentd/work/[プロジェクト名]` にて初期資材を生成します。

1.  **ディレクトリ作成と所有権設定**:
    ```bash
    sudo mkdir -p /opt/agentd/work/[プロジェクト名]
    sudo chown agent:agent /opt/agentd/work/[プロジェクト名]
    ```
2.  **Vite 初期化（agent ユーザ）**:
    ```bash
    sudo -u agent bash -c "cd /opt/agentd/work/[プロジェクト名] && npm create vite@latest . -- --template vanilla-ts -y && npm install"
    ```
3.  **主要ライブラリ・ディレクトリ構成の導入**:
    - `phaser`, `vite-plugin-pwa` 等、マニフェスト（`INITIAL_MANIFEST/04_REPO_TEMPLATE_WEB_GAME.md`）に準拠した構成を適用。
    - `src/game`, `src/app`, `src/services` 等のディレクトリを生成。

## 4. Git 初期設定と初回プッシュ

1.  **Git Init & Commit**: 
    - `git init`, `git branch -m main`
    - 初回の `.gitignore` 設定、コミット。
2.  **リモート追加とプッシュ**:
    - `git remote add origin git@github.com:AIBoy/[プロジェクト名].git`
    - `git push -u origin main`

## 5. プラットフォーム（agentd）への登録

- **config.json の更新**: 
    - `/opt/agentd/config.json` に新プロジェクトの SSH/HTTP URL を追加。
- **Nginx 確認**: 
    - `INITIAL_MANIFEST/02_SERVER_BOOTSTRAP.md` の **「Nginx 設定見本」** が反映されていることを確認。
    - `https://[ドメイン名]/[プロジェクト名]/` で疎通ができるか（動的ルーティングが機能しているか）を確認。

---

> [!IMPORTANT]
> **環境の区別について**:
> - このディレクトリ配下にあるファイルは、ローカル（構築作業用）の資材です。
> - このディレクトリ配下に存在しないパス（例: `/opt/agentd`, `/etc/nginx`, `/var/www` 等）は、すべて **「外部サーバ（構築対象）」** 上のディレクトリを指します。
> - ローカル環境と外部サーバ環境を混同しないように注意してください。

## 6. AI エージェントへのハンドオーバー

構築の最終ステップとして、GitHub Issue を作成し、AI エージェントに具体の開発指示を投げます。

- **Issue 起票**: 
    - `gh issue create --title "Initial Development" --body "@agent run: [詳細な指示]"`
- **ログ記録**: 
    - `log/[YYYYMMDD]_[プロジェクト略称].log` に構築の全工程を記録。
    - **重要**: 作業開始時にログファイルを作成し、思考・試行・結果をリアルタイムで詳細に書き出すこと（`AI_INSTRUCTIONS.md` に準拠）。

---
**更新履歴**:
- 2026-02-21: 初版策定（game01_roguelike の構築実績に基づく）
