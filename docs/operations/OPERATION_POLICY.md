# LibreChat 運営方針

> kasai-oot/LibreChat フォーク運営の基本方針

## 概要

本ドキュメントはLibreChat（kasai-ootフォーク）の長期運営における基本方針を定める。

## アップストリーム同期方針

| 項目 | 方針 |
|------|------|
| 同期頻度 | 週次（毎週月曜 GitHub Actions自動実行） |
| 同期方式 | rebase（コミット履歴を直線的に保つ） |
| コンフリクト解決 | syncwright（AI自動解決）→ 失敗時は手動 |
| 同期ブランチ | `upstream-sync/YYYY-MM-DD` → PR → `prod-railway` |

## デプロイ方針

| 項目 | 方針 |
|------|------|
| 本番環境 | Railway (`prod-railway` ブランチ連動) |
| デプロイトリガー | PRマージ時に自動デプロイ |
| デプロイ通知 | Discord #claude-kasai に完了通知 |
| ロールバック | Railway ダッシュボードから前バージョンに復元 |

## RACI

| 作業 | Responsible | Accountable | Consulted | Informed |
|------|------------|-------------|-----------|----------|
| アップストリーム同期PR作成 | kasai Claude Code (自動) | hidekasai | - | Discord通知 |
| PRレビュー・マージ | hidekasai | hidekasai | kasai Claude Code | - |
| カスタマイズ変更 | 開発担当者 | hidekasai | kasai Claude Code | Discord通知 |
| ユーザー管理 | hidekasai | hidekasai | - | - |
| セキュリティ対応 | kasai Claude Code | hidekasai | - | Discord緊急通知 |

## セキュリティ方針

- 本番環境のシークレット（APIキー等）はRailway環境変数で管理
- シークレットをリポジトリにコミットしない
- 依存パッケージの脆弱性はDependabotで監視（TODO: 設定）
- アップストリームのセキュリティ修正は優先的に取り込む

## ブランチ戦略

```
upstream/main
    ↓ (週次自動同期)
upstream-sync/YYYY-MM-DD  ← GitHub Actions が自動作成
    ↓ (PR + レビュー)
prod-railway  ← Railway本番デプロイ連動
```

## 変更履歴

| 日付 | 内容 | 担当 |
|------|------|------|
| 2026-03-26 | 初版作成 | kasai Claude Code |
