# 01_SERVER_OS_GUIDE.md（OS選定）
責務種別: `BUILD`

## 何を行うのか
- VPSスペック制約（特に低メモリ）に合ったOS選定方針を定義する
- 常駐エージェント、systemd、Node/Python、GitHub連携に適した運用しやすいOSを採用する
- 低メモリ環境での運用前提（swap追加、重い処理のCI寄せ）を明示する

## 選定方針
- 第一候補は Debian 系（情報量・安定性・軽量性・運用経験を優先）
- 代替候補として AlmaLinux 系を扱う
- どちらを選ぶ場合でも、以降の章の目的（agentd常駐、Webhook、CI連携）を満たすことを優先する

## 判断基準
- systemd 常駐運用のしやすさ
- Node / Python / Git / gh CLI / Webサーバの導入容易性
- メモリ制約下での安定稼働性
- 将来の保守性と情報量
