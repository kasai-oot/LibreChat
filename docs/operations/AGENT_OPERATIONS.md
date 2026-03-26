# LibreChat エージェント運営ガイド

> AIエージェントによる自動化範囲・役割・エスカレーション手順

## エージェント役割分担

| エージェント | マシン | 担当範囲 |
|------------|--------|---------|
| **kasai Claude Code** | q4os | LibreChat全体の運営管理・判断・Discord報告 |
| **Codex** | q4os | コード実装・PR作成・ドキュメント更新 |
| **Gemini** | q4os | 調査・分析・ドキュメント設計 |
| **trixie Claude Code** | trixie | 長時間タスク・並列処理 |

## 自動化範囲

### 自動実行（人間の承認不要）

| タスク | 頻度 | 担当 |
|--------|------|------|
| アップストリーム差分チェック | 週次（月曜） | GitHub Actions |
| 同期PRの自動作成 | 差分検知時 | GitHub Actions + syncwright |
| Railway自動デプロイ | PRマージ時 | Railway |
| Discord完了通知 | デプロイ完了時 | GitHub Actions |

### 要承認（hidekasaiのレビュー必須）

| タスク | 承認方法 |
|--------|---------|
| 同期PRのマージ | GitHub PRレビュー |
| カスタマイズ変更 | GitHub PRレビュー |
| 本番環境設定変更 | Discord相談後に実施 |
| 新規ユーザー追加 | LibreChat管理画面で直接操作 |

## エスカレーション手順

### コンフリクト自動解決失敗時

```
GitHub Actions → syncwright失敗
    ↓
Discord #claude-kasai に通知
    「⚠️ LibreChatアップストリーム同期でコンフリクト。手動対応が必要です。」
    ↓
hidekasaiが確認・kasai Claude Codeに指示
    ↓
kasai Claude Code + Codexで手動解決
    ↓
PR作成・レビュー・マージ
```

### デプロイ失敗時

```
Railway デプロイ失敗
    ↓
Discord #claude-kasai に通知
    ↓
kasai Claude Code がログを確認・原因特定
    ↓
Codexに修正を依頼 or hidekasaiに報告
    ↓
修正PR → マージ → 再デプロイ
```

### セキュリティ問題発生時

```
脆弱性発見（Dependabot等）
    ↓
Discord #claude-kasai に緊急通知
    ↓
hidekasaiが優先度判断
    ↓
高優先度: 即時パッチ適用
低優先度: 次回同期サイクルに含める
```

## 運営状況ダッシュボード

- **Railway**: デプロイ状況・ログ確認
- **Discord #claude-kasai**: 運営通知・エラー報告
- **GitHub Actions**: 自動化タスクの実行履歴
- **GitHub PR**: 同期・変更のレビュー待ち一覧

## TODO（今後整備予定）

- [ ] GitHub Actions: 週次アップストリーム同期ワークフロー
- [ ] syncwright: コンフリクト自動解決の設定
- [ ] Dependabot: 依存パッケージ脆弱性監視
- [ ] ステージング環境: 本番前テスト環境の構築
- [ ] 監視・アラート: サービスダウン検知

## 変更履歴

| 日付 | 内容 | 担当 |
|------|------|------|
| 2026-03-26 | 初版作成 | kasai Claude Code |
