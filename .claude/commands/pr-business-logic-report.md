---
description: PR番号から関連機能を特定し、その機能のビジネスロジックを詳細に調査してレポートを生成
allowed-tools: Bash(git diff:*), Bash(git log:*), Bash(git show:*), Bash(gh:*), Read, Write, Grep, Glob, Task
argument-hint: <pr-number>
---

## 概要

PR番号を引数として受け取り、以下を実施するコマンドです：

1. PRの差分から**関連機能（ユースケース）を特定**（これはきっかけに過ぎない）
2. **特定した機能全体のビジネスロジックを網羅的に調査**（PR変更内容の詳細は調査範囲の決定に影響しない）
3. 調査結果を**構造化されたレポートとして出力**

**特徴**:
- PRの差分だけでなく、関連する機能全体のビジネスロジックを網羅的に調査
- データ構造、定数定義、バリデーション、ビジネスルールを体系的に分析

## 前提条件

- PR番号を引数で指定すること
- GitHubリポジトリへのアクセス権限があること（`gh` CLIが設定済み）
- ビジネスロジックレポート生成ルールが定義されていること（`~/.claude/policies/pr-business-logic-report.md`）
- テンプレートが存在すること（`~/.claude/policies/templates/pr-business-logic-report-template.md`）

## 引数（$ARGUMENTS）

- **必須**: `<pr-number>` - 調査対象のPR番号

例：
- `/pr-business-logic-report 107`
- `/pr-business-logic-report 123`

## 実行手順

### 1. ルールとテンプレートの読み込み

以下のファイルを読み込むこと（**いずれかが見つからない場合はエラーを表示して処理を停止**）：

1. **Read `~/.claude/policies/pr-business-logic-report.md`**
   - ビジネスロジックレポート生成ルール（調査原則・分析方法の詳細）

2. **Read `~/.claude/policies/templates/pr-business-logic-report-template.md`**
   - 出力フォーマットのテンプレート

### 2. PR情報の取得

```bash
# PRの基本情報を取得
gh pr view <pr-number> --json title,body,baseRefName,headRefName,url,number

# PRの差分を取得
gh pr diff <pr-number>
```

以下の情報を抽出：
- PRタイトル・説明
- ベースブランチ・ヘッドブランチ
- 変更ファイル一覧
- 関連Issue番号（あれば）

### 3. 機能（ユースケース）の特定

**ルールファイルのセクション0.2「調査スコープの決定方法」に従って**、PRから関連する機能（ユースケース）を特定する。

### 4. ビジネスロジック調査の実施

**ルールファイルのセクション0.3〜セクション4に従って**、以下を実施：

- 関連コードの網羅的探索
- データ構造の分析
- 定数・Enum体系の分析
- バリデーションロジックの詳細分析
- ビジネスルールの詳細分析
- エラーメッセージの抽出
- 影響範囲の特定

### 5. レポートの出力

**出力先**: `[プロジェクトのドキュメントディレクトリ]/pr-business-logic-reports/pr-[PR番号]-report.md`

> **注**: 出力先フォルダが存在しない場合は作成すること。プロジェクトの慣例に従い、適切なディレクトリを選択すること（例: `docs/`, `documentation/`等）。

**出力フォーマット**: テンプレートファイルの構造を**完全に遵守**すること。

### 6. 生成後の自己チェック

**ルールファイルのセクション5「網羅性検証の原則」に従って**、調査の網羅性を検証し、結果を出力する。

## エラーハンドリング

### PRが見つからない場合

```
❌ エラー: PR #[番号] が見つかりません

対処方法：
1. PR番号が正しいか確認してください
2. GitHubリポジトリへのアクセス権限を確認してください
3. `gh auth status` でGitHub CLIの認証状態を確認してください
```

### ルールファイル/テンプレートが見つからない場合

```
❌ エラー: ビジネスロジックレポート生成ルール/テンプレートが見つかりません

検索したパス: ~/.claude/policies/pr-business-logic-report.md
             ~/.claude/policies/templates/pr-business-logic-report-template.md

対処方法：
1. ファイルが正しい場所に配置されているか確認してください
```

## 注意事項

- **ルールの遵守**: `~/.claude/policies/pr-business-logic-report.md`のルールを厳守
- **テンプレート使用**: `~/.claude/policies/templates/pr-business-logic-report-template.md`のフォーマットに従う
- **出力先の統一**: プロジェクトのドキュメントディレクトリ配下の`pr-business-logic-reports/`に保存
