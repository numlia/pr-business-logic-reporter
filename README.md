# PRビジネスロジックレポートのカスタムコマンド

## 概要

この機能は、PR番号から関連機能を特定し、その機能のビジネスロジックを詳細に調査してレポートを生成するClaudeCodeカスタムコマンドです。
その他のAIエージェントで利用する場合は、コマンドのフロントマターやファイル参照パスを適切に変更する必要があります。

## ファイル構成

PRビジネスロジックレポート機能は、以下の3つのファイルで構成されています：

### 1. コマンド定義ファイル

**配置**: `~/.claude/commands/pr-business-logic-report.md`

**使用方法**:
```bash
/pr-business-logic-report <pr-number>
```

### 2. ルールファイル

**配置**: `~/.claude/policies/pr-business-logic-report.md`

**役割**: ビジネスロジックレポート生成時の原則とルールを定義

### 3. テンプレートファイル

**配置**: `~/.claude/policies/templates/pr-business-logic-report-template.md`

**役割**: 出力するレポートの統一フォーマット

## ファイル間の関係

```
~/.claude/commands/pr-business-logic-report.md
  ├─ 読み込み → ~/.claude/policies/pr-business-logic-report.md
  │             (ビジネスロジックレポート生成ルール)
  │
  └─ 読み込み → ~/.claude/policies/templates/pr-business-logic-report-template.md
                (出力フォーマットのテンプレート)

出力先: docs/pr-business-logic-reports/pr-[PR番号]-report.md
```
