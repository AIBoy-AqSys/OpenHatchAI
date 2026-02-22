# README.md
（External Server AI Agent Manifest / Setup Guide）

このプロジェクトは、VPS上に **常駐AIエージェント（agentd）** を構築し、GitHub Issue コメントを起点に  
**自律的な修正↔検証ループ** を回して PR を作成できるようにするためのマニフェスト群です。

---

## このプロジェクトでできること（目標）
- GitHub Issue コメントをトリガにジョブ実行
- 実行前に `main` を最新化
- 作業ブランチを作成
- 自律的に修正と検証を反復
- コード検証に加えてブラウザ検証を積極活用
- 問題解決時に PR 作成
- 進捗 / 完了 / エラーを Issue に通知
- AI Provider を `codex` / `claude` / `gemini` で切替可能

---

## 大事な考え方（混乱防止）
このマニフェストは、次の2つが合体しています。

1. **BUILD（サーバをどう構築するか）**
2. **RUNTIME_POLICY（サーバ側エージェントがどう動くべきか）**

つまり、最初にやるべきことは「Issueを解かせること」ではなく、  
**Issueを解けるサーバ基盤を構築すること**です。

---

## ディレクトリ構成（想定）
- `00_OVERVIEW.md` ～ `12_SERVER_AI_INSTRUCTIONS.md`：マニフェスト本体
- `.env_sample` / `.env_sample_extended_for_manifest.txt`：環境変数サンプル
- `BUILD_AGENT_PROMPT.md`：構築側エージェント専用の実行プロンプト（BUILDのみ）
- （実装側）agentd 本体、Webhook受信処理、ジョブ処理、ブラウザ検証実行基盤 など

---

## まずユーザーがやること（人間作業）
サーバ側エージェントを稼働可能にするには、まずユーザーが以下を用意します。

### 1) VPSを用意する
- Debian系推奨（代替でAlmaLinux系も可）
- SSH接続できる状態にする
- ドメイン / DNS / HTTPS を使うなら、その前提も準備する

### 2) GitHub側の準備をする
- 対象リポジトリを用意する
- ブランチ保護やCIを適用する対象を決める
- Webhook を設定できる権限を用意する
- GitHub CLI / SSH 認証に使うアカウント・鍵方針を決める

### 3) APIキー・設定値を用意する（重要）
`.env` に投入するため、少なくとも以下のうち使うものを用意します。

- `CODEX_API_KEY`
- `CLAUDE_API_KEY`
- `GEMINI_API_KEY`

加えて、必要に応じて：
- Webhook署名用のシークレット
- ブラウザ検証用のベースURL
- Provider既定値（`AI_PROVIDER_DEFAULT`）
- ブラウザ検証有効/無効設定 など

### 4) `.env` を作成する
- `.env_sample` または拡張版 `.env_sample_extended_for_manifest.txt` を元に `.env` を作成
- **空欄でも良いキーは残す**（存在を宣言しておく）
- 秘密情報はログに出ないよう管理する

---

## サーバ側エージェントを稼働可能にするまでの流れ（ユーザー視点）

### Step 1. マニフェストを配置する
`00_` ～ `12_` のマニフェストファイルを作業用リポジトリまたは管理場所に配置します。

特に重要なのは：
- `00_OVERVIEW.md`
- `03_AGENTD_DAEMON.md`
- `11_CHECKLIST.md`
- `12_SERVER_AI_INSTRUCTIONS.md`

### Step 2. 構築側エージェントに BUILD だけ実行させる
`BUILD_AGENT_PROMPT.md` を使って、構築側エージェントに以下をさせます。

- VPS初期構成
- agentd実行基盤導入
- Webhook/ジョブ/ログ/常駐化前提の整備
- ブラウザ検証基盤の導入
- Provider切替設定基盤の導入
- `12_SERVER_AI_INSTRUCTIONS.md` のサーバ配置
- `11_CHECKLIST.md` による導入確認

> ポイント: この段階では「Issue解決の実運用」はまだ主目的ではありません。  
> まずは **基盤構築と導入確認** がゴールです。

### Step 3. BUILD_VERIFICATION の結果を確認する
構築側エージェントの最終報告で、最低限以下を確認します。

- agentd が常駐可能
- Webhook受信前提が整っている
- `main` 最新化前提を扱える構成
- ブラウザ検証基盤が agentd から利用可能
- Provider切替設定を読める
- `12_SERVER_AI_INSTRUCTIONS.md` の配置・参照が可能
- 未投入の必須APIキー等が整理されている

### Step 4. 未投入の秘密情報を入れる（人間作業）
構築側エージェント報告を見て、未投入の値を `.env` に追加します。

例：
- `CODEX_API_KEY`
- `CLAUDE_API_KEY`
- `GEMINI_API_KEY`
- Webhook シークレット
- Browser base URL など

### Step 5. 小さなIssueで試験運用する
最初は小さな修正Issueで、常駐エージェントの流れを確認します。

確認観点：
- Issueコメント起動 → ジョブ化
- `main` 最新化 → ブランチ作成
- 修正 → 検証（必要ならブラウザ）
- PR作成
- 進捗/完了/エラー通知

---

## どのファイルをどう読めばよいか（ユーザー向け）

### 初見で読む順番（人間）
1. `00_OVERVIEW.md`（全体像）
2. `README.md`（このファイル）
3. `02_SERVER_BOOTSTRAP.md`（VPS初期構成）
4. `03_AGENTD_DAEMON.md`（agentdの役割）
5. `12_SERVER_AI_INSTRUCTIONS.md`（サーバ側エージェントの運用基盤）
6. `11_CHECKLIST.md`（導入確認観点）
7. `.env_sample_extended_for_manifest.txt`（必要キー確認）

### 構築側エージェントに読ませる順番
- `BUILD_AGENT_PROMPT.md` を最初に渡す
- 次に `00`, `01`, `02`, `03`, `06`, `08`, `09`, `12`, `11`, `.env_sample...` を読ませる

---

## `.env` で特に重要な項目（抜粋）
- `AI_PROVIDER_DEFAULT`
- `AI_PROVIDER_FALLBACK_ORDER`
- `CODEX_API_KEY`
- `CLAUDE_API_KEY`
- `GEMINI_API_KEY`
- `BROWSER_VERIFY_ENABLED`
- `BROWSER_VERIFY_MODE`
- `BROWSER_BASE_URL`
- `AGENT_LOOP_MAX_ATTEMPTS`
- `AGENT_LOOP_MAX_SAME_FAILURE`
- `GIT_SYNC_MAIN_BEFORE_BRANCH`
- `GIT_MAIN_BRANCH_NAME`

---

## サーバ側エージェントが混乱しないためのポイント
- `12_SERVER_AI_INSTRUCTIONS.md` をサーバ上に配置し、agentd 実行コンテキストから参照可能にする
- Provider を切り替えても、**成功条件・停止条件・報告形式は共通**にする
- ブラウザ検証は「導入済み」だけでなく、Issue解決の実判断に使う
- 停止条件を明確にして、無限反復を避ける

---

## トラブル時によくある詰まりどころ
- `.env` に必須キー不足（APIキー / Webhook秘密 / Browser base URL）
- GitHub権限不足（Webhook設定 / PR作成 / gh認証）
- ブラウザ検証基盤は入ったが agentd から呼べない
- `12_SERVER_AI_INSTRUCTIONS.md` を配置したが参照パス未設定
- BUILD と RUNTIME_POLICY を混同して、構築段階で実運用Issueを解こうとしてしまう

---

## 進め方のおすすめ
- 最初は **単一リポジトリ + 小さいIssue** で試す
- Providerは最初に1つ固定し、後から切替を有効化する
- ブラウザ検証は UI系Issue から優先的に使う
- `11_CHECKLIST.md` を毎回の再構築/更新時にも使う

---

## 最後に
このプロジェクトの本質は、単なるサーバ構築ではなく、

- **構築側エージェントが迷わないこと**
- **サーバ側常駐エージェントが迷わないこと**
- **人間ユーザーが、どこで何をすべきか分かること**

を同時に満たすことです。

この README はそのための入口として使ってください。
