# PROJECT_BOOTSTRAP_ON_VPS.md（VPS上で新規プロジェクトを起こす手順）

本ドキュメントは、ターゲットVPS上で **agentd 管理下の新規プロジェクト（Webゲーム想定）** を起こすための「手順レシピ」です。

- **正典（何を作るべきか / どんな構造にするべきか）**：`manifests/04_REPO_TEMPLATE_WEB_GAME.md`
- **このドキュメント（どう起こすか / 手順の段取り）**：本ファイル

> [!IMPORTANT]
> OSやディストリビューションは変わり得るため、ここでは **コマンドを固定しません**。
> 実務（具体コマンドの選択・実行）は構築済みの常駐エージェント（agentd など）に委譲し、
> ここでは **手順の段取り / 成功条件 / 参照先** だけを明示します。

---

## 1. 事前確認（Pre-flight）

プロジェクト作成を始める前に、サーバ側の前提を確認します。

- [ ] **OS方針が確定**：`manifests/01_SERVER_OS_GUIDE.md` の判断ができている
- [ ] **基盤構築が完了**：`manifests/02_SERVER_BOOTSTRAP.md` の BUILD が完了している
  - Node.js / npm が利用可能（導入方式は OS により異なるのでエージェントが選択）
  - Git / Python / 主要ユーティリティが利用可能
- [ ] **GitHub 認証が確立**：`manifests/06_GITHUB_GATES.md` の前提に沿って、
  - gh CLI で認証できる、または SSH で clone/push が可能
- [ ] **作業ディレクトリが用意済み**：`manifests/03_AGENTD_DAEMON.md` のパス設計に従い、
  - agentd が扱うワークディレクトリ配下に、プロジェクト作成ができる

---

## 2. GitHub リポジトリの準備

- [ ] **命名規則（例）**：`game01_roguelike` のように小文字＋スネークケース推奨
- [ ] **リポジトリ作成**：手段は問わない（Web UI / gh CLI / API）
  - 推奨：最初は「空のリポジトリ」で作成し、サーバ側で初期化して push する（衝突回避）

---

## 3. サーバ側ブートストラップ（初期化）

作業場所（例：`/opt/agentd/work/[project]`）やユーザ（例：`agent`）は、
`manifests/03_AGENTD_DAEMON.md` の設計を正とします。

### 3.1 作業ディレクトリの用意

- [ ] プロジェクト用ディレクトリを作成する
- [ ] agentd 実行ユーザが読み書きできる（所有権・権限）

### 3.2 フロントエンド初期化（Vite + TypeScript を基本）

- [ ] `vite` ベースで TypeScript テンプレを初期化する
- [ ] 依存解決まで完了し、ローカルビルドが通る状態にする

> [!NOTE]
> 具体的なコマンドは OS / Node導入方式 / 権限設計により変わります。
> そのためここでは固定せず、エージェントが `manifests/02_SERVER_BOOTSTRAP.md` と
> `manifests/03_AGENTD_DAEMON.md` を参照して実行します。

### 3.3 テンプレ構成の適用（正典はマニフェスト）

- [ ] `manifests/04_REPO_TEMPLATE_WEB_GAME.md` に沿ってディレクトリ構成を作る
- [ ] 依存ライブラリも、マニフェストの方針に合わせて追加する

---

## 4. Git 初期設定と初回 push

- [ ] 作業ディレクトリを Git リポジトリとして初期化
- [ ] `main` ブランチで初回コミットを作成
- [ ] GitHub リポジトリを `origin` に設定
- [ ] `main` を push（認証手段は環境に合わせる：gh/SSH）

---

## 5. agentd への登録とブラウザ疎通

- [ ] **リポジトリ登録**：`manifests/03_AGENTD_DAEMON.md` の「対象リポジトリ管理（レジストリ形式）」に従って登録
- [ ] **Web公開**：`manifests/02_SERVER_BOOTSTRAP.md` の方針に従って疎通確認
  - 例：`https://[domain]/[project]/`（実際の公開URLは構成に依存）

---

## 6. AI エージェントへのハンドオーバー（任意）

最終ステップとして、GitHub Issue を作成し、常駐エージェントに「次の作業」を渡します。

- [ ] Issue の本文に、**参照すべき正典（マニフェスト）** を明示する
  - 例：`manifests/04_REPO_TEMPLATE_WEB_GAME.md`
  - 例：PWA / app-shell / services 分離などの方針を明示

> [!IMPORTANT]
> **環境の区別**：
> - このリポジトリ（OpenHatchAI）配下のパスはローカルの設計資材
> - `/opt/agentd`, `/etc/nginx`, `/var/www` 等はターゲットVPS上の実パス
> - ローカルとVPSの混同が起きると、事故の原因になります

---
