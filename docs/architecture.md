# プロジェクト概要・アーキテクチャ

## 1. 開発の目的

TypeScript およびモダンなフルスタックWeb開発エコシステム（React, Edgeランタイム等）の学習。
自動生成に頼りきらず、ステップバイステップで仕組みを理解しながら構築・実装を進める。

## 2. 技術スタック

| カテゴリ            | 採用技術                    | 選定理由・備考                                        |
| :------------------ | :-------------------------- | :---------------------------------------------------- |
| **Framework**       | TanStack Start (React)      | Viteベース、高度な型安全ルーティング                  |
| **UI / Styling**    | shadcn/ui + Tailwind CSS v4 | 美しくカスタマイズ性の高いコンポーネント              |
| **Database**        | Cloudflare D1               | サーバーレスエッジDB (ローカル環境はSQLiteとして動作) |
| **ORM**             | Drizzle ORM                 | 高速で型安全なSQLクエリビルダー                       |
| **Package Manager** | pnpm                        | ディスク容量に優れ、高速で厳格なパッケージ管理        |
| **Runtime**         | Node.js (v24 LTS)           | 最新の安定版バックエンドランタイム                    |

## 3. 開発環境 (DevContainer)

Docker Compose をベースにした VS Code DevContainers 環境を採用。ホストマシンの環境を汚さずに開発が可能。

- **ポート競合の回避**: インフラ用設定として `.devcontainer/.env` を利用し、ホスト側の公開ポートを開発者単位でずらせるように設計。
- **柔軟な個別拡張**: `.devcontainer/compose.override.yaml` （`.gitignore` で管理外）を用いて、開発者ごとのローカルマウントや認証情報の追加を可能にしている。
- **使用ポート一覧 (コンテナ内)**:
  - Webアプリ (TanStack Start): `3000` (※起動時は `--host` 指定必須)
  - Drizzle Studio (DB管理GUI): `4983` (※起動時は `--host` 指定必須)
  - Cloudflare Wrangler: `8787`

## 4. ブランチ戦略 (GitHub Flow ベース)

チーム開発を見据えた安全なブランチ運用を行う。

- **`main` ブランチ**:
  - 常にデプロイ可能で安定した本番（プロダクション）用ブランチ。直接のPushは行わない。
- **`develop` ブランチ (既定ブランチ)**:
  - 開発のベースとなる統合先。GitHub上のDefault Branch。
- **`feature/*` ブランチ**:
  - 実際の作業（機能追加・設定変更）を行うブランチ。必ず `develop` から切り出し、作業完了後にPull Request (Merge) して `develop` へ取り込む。
  - 例: `feature/setup-shadcn`, `feature/create-users-table` など
