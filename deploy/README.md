# deploy/README.md

この配下は「サーバへ配置するファイル」を置く場所です。

## server-files/
- サーバ側で常駐エージェントが参照する指示書・プロトコル類を配置します。
- `manifests/12_SERVER_AI_INSTRUCTIONS.md` はサーバ側の基盤指示書として配置候補です。
- `AI_INSTRUCTIONS.md` / `AI_PROJECT_BOOTSTRAP_PROTOCOL.md` は現状は転用版（後で差し替え想定）。

## 目的
構築側AIが「ローカル設計資料」と「サーバに配置する実体」を混同しないようにすること。
