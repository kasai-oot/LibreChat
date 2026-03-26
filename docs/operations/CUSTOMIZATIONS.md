# LibreChat カスタマイズ差分管理

> アップストリームからの独自変更を記録・管理するドキュメント

## 目的

アップストリーム同期時のコンフリクト回避と、カスタマイズ意図の保存のため、全ての独自変更をここで管理する。

## 現在のカスタマイズ一覧（prod-railwayブランチ）

### 1. アイコン・ロゴ変更（ブランディング）

| ファイル | 変更内容 | 同期時の注意 |
|---------|---------|------------|
| `client/public/assets/favicon-16x16.png` | カスタムfavicon (16x16) | アップストリーム更新不要（独自ブランド維持） |
| `client/public/assets/favicon-32x32.png` | カスタムfavicon (32x32) | 同上 |
| `client/public/assets/icon-192x192.png` | PWAホーム画面アイコン | 同上 |
| `client/public/assets/logo.svg` | LibreChatオリジナルSVGをブランドロゴ（PNG埋め込みSVG）に差し替え | アップストリームのSVG更新は無視してよい |

**変更理由**: kasai-ootブランド向けUI

---

### 2. フッター変更・法的文書表示

| ファイル | 変更内容 | 同期時の注意 |
|---------|---------|------------|
| `client/src/components/Chat/Footer.tsx` | フッターを非表示、X3D法的文書（ポリシー・ToS）を動的表示するよう変更 | アップストリームのフッター変更と手動マージが必要 |
| `client/src/components/ui/Dialog.tsx` | 法的文書表示用のDialogコンポーネント（新規追加） | アップストリームに同名ファイルが追加された場合は内容確認 |
| `api/server/services/Config/loadCustomConfig.js` | `config/x3d-policy.md` と `config/x3d-tos.md` を動的ロードする機能を追加 | アップストリームの変更と手動マージが必要 |

**変更理由**: 日本法準拠のプライバシーポリシー・利用規約をUIに表示するため

---

### 3. 独自法的文書

| ファイル | 変更内容 | 同期時の注意 |
|---------|---------|------------|
| `config/x3d-policy.md` | X3D向けプライバシーポリシー | アップストリームに存在しないファイル。衝突なし |
| `config/x3d-tos.md` | X3D向け利用規約 | 同上 |

**変更理由**: 日本法・X3Dブランド向け法的文書

---

### 4. librechat.yaml（X3D・Bedrockモデル設定）

| ファイル | 変更内容 | 同期時の注意 |
|---------|---------|------------|
| `librechat.yaml` | X3D-Agents向けエンドポイント、Bedrock東京リージョン（ap-northeast-1）、カスタムモデル設定 | アップストリームの `librechat.yaml.example` と差分確認が必要 |

**変更理由**: X3Dクライアント向けカスタムAIエージェント提供、日本ユーザー向けレイテンシ最適化

---

### 5. Dockerfile（Railway向け調整）

| ファイル | 変更内容 | 同期時の注意 |
|---------|---------|------------|
| `Dockerfile` | `librechat.yaml` と `config/` ディレクトリのCOPY追加、bun.lockb削除（npm使用）、Railway向けポート設定 | アップストリームのDockerfile変更を手動マージ |
| `.gitignore` | Railway関連ファイルの除外設定 | 内容確認して必要に応じてマージ |

**変更理由**: Railway PaaS環境へのデプロイ最適化

---

### 6. 独自ドキュメント

| ファイル | 変更内容 | 同期時の注意 |
|---------|---------|------------|
| `GEMINI.md` | Gemini CLI連携ドキュメント | アップストリームに存在しないファイル。衝突なし |
| `WARP.md` | WARP設定ドキュメント | 同上 |

**変更理由**: 内部運営ドキュメント

---

## カスタマイズ変更時のルール

1. このドキュメントを必ず更新する
2. 変更理由を明記する（将来の自分や他の担当者への引き継ぎ）
3. アップストリーム同期時のリスクを評価して記載する
4. 大きな変更はブランチを切ってPRでレビュー

## アップストリーム同期時のチェックリスト

### 自動解決できる可能性が高い（コンフリクトなし）
- [ ] `config/x3d-policy.md` — アップストリームに存在しないため安全
- [ ] `config/x3d-tos.md` — 同上
- [ ] `GEMINI.md`, `WARP.md` — 同上
- [ ] アイコン・ロゴ (`client/public/assets/`) — バイナリファイルのため手動確認

### 手動マージが必要な可能性が高い
- [ ] `librechat.yaml` — アップストリームの `librechat.yaml.example` と差分確認
- [ ] `Dockerfile` — アップストリームの変更内容を確認してマージ
- [ ] `client/src/components/Chat/Footer.tsx` — アップストリームのフッター変更と照合
- [ ] `api/server/services/Config/loadCustomConfig.js` — アップストリームの変更と照合
- [ ] `client/src/components/ui/Dialog.tsx` — アップストリームに同名ファイル追加がないか確認

### 動作確認
- [ ] ブランド表示（ロゴ・favicon）が正しく表示される
- [ ] フッターのポリシー・ToSリンクが機能する
- [ ] X3Dエージェントのエンドポイントが動作する
- [ ] Bedrock東京リージョンが正常に動作する
- [ ] Railwayデプロイが成功する

## 変更履歴

| 日付 | 内容 | 担当 |
|------|------|------|
| 2026-03-26 | 初版作成（prod-railwayブランチの全カスタムコミットを調査して記録） | kasai Claude Code |
