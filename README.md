# DuckDeck Agent

**DuckDeck Agent は、Linuxサーバー内の状態情報（CPU、メモリ、ディスク、ネットワーク、プロセスなど）を定期収集し、DuckDeck Backend に送信する軽量クライアントツールです。**

このリポジトリは、サーバー可視化・ログ分析プラットフォーム「[DuckDeck](https://github.com/sakurafactory-oss/duckdeck-backend)」の一部として設計されています。

---

## 特徴

- サーバーの各種情報を JSON 形式で収集
- DuckDeck Backend に HTTP 経由で定期送信
- cron / systemd-timer に組み込みやすい単体CLI
- Go製・依存最小・クロスプラットフォーム対応
- 環境に応じて取得項目のカスタマイズが可能

---

## 取得対象情報（初期対応）

| カテゴリ       | 項目例 |
|----------------|--------|
| **CPU**        | 使用率, コア数, ロードアベレージ |
| **メモリ**     | 使用量, 空き容量, スワップ状況 |
| **ディスク**   | 空き容量, マウントポイント, 使用率 |
| **ネットワーク** | アクティブポート, 転送量 |
| **プロセス**   | 実行中プロセス数, Topプロセス（CPU/MEM） |
| **セッション** | ログインユーザー数, SSHセッション状態 |
| **ホスト情報** | ホスト名, IP, OS, uptime |

---

## 実行方法

### 1. バイナリ単体実行

```bash
./duckdeck-agent --endpoint http://localhost:8080/api/ingest/metrics
```

2. crontab 登録例（5分毎）

*/5 * * * * /usr/local/bin/duckdeck-agent --endpoint http://localhost:8080/api/ingest/metrics



⸻

実行例（送信されるJSON）

{
  "timestamp": "2025-04-17T01:05:00Z",
  "hostname": "web-server-01",
  "cpu_percent": 67.2,
  "mem_used_mb": 5984,
  "load_avg": 1.22,
  "disk_free_gb": 84.3,
  "active_sessions": 3
}



⸻

使用技術
	•	Go（クロスコンパイル可、バイナリ単体）
	•	OS情報取得に gopsutil 使用
	•	HTTPクライアント標準ライブラリで送信

⸻

インストール

go build -o duckdeck-agent cmd/main.go

または GitHub Releases よりバイナリをダウンロードして配置：

sudo cp duckdeck-agent /usr/local/bin/
chmod +x /usr/local/bin/duckdeck-agent



⸻

カスタマイズ項目

オプション	内容
--endpoint	送信先のAPIエンドポイント
--interval	実行間隔（自動ループモード用、例: 60s）
--config	外部設定ファイル（将来対応予定）



⸻

ライセンス

Apache License 2.0

⸻

開発者向け
	•	軽量サーバー情報収集ツールとして、CLIツール単体でも使用可能
	•	自作監視システムや社内統計基盤との連携も可能
	•	コントリビューション・Issue報告歓迎！

---
