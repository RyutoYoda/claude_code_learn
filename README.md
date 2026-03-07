# Claude Code ハンズオン入門リポジトリ

テーマは **「Streamlitでデータ可視化アプリを作る」**。  
`workshop/` 以下の1ファイルで、インストール〜コマンド実行〜デモアプリ作成まで体験できます。

## 0. 30分ハンズオンの到達目標

- Homebrewから Claude Code をインストールし、起動できる
- `/help` 系のスラッシュコマンド（`/compact`, `/agents`, `/skills`, `/status`）を実際に使う
- Streamlit用Skillを導入し、`/skills` で運用する
- 10〜15分で最小のデータ可視化アプリを作る
- このハンズオン中は、インストール・`/login`・`/`コマンド以外のCLI実行はしない想定

## 1. インストールと起動

### 1-1. インストール（OS別）

macOS / Linux / WSL（推奨）  
```bash
brew install --cask claude-code
```

Windows (PowerShell)  
```powershell
irm https://claude.ai/install.ps1 | iex
```

または  
```bash
winget install Anthropic.ClaudeCode
```

他の導線で入っている場合は、この手順を飛ばして起動へ進んでOK

### 1-2. 起動と認証

```text
claude
```

初回は `/login` を実行して認証してください。

### 1-3. 起動直後に叩くコマンド

- `/help`
- `/help compact`
- `/compact 会話を短く整理`
- `/skills`
- `/agents`
- `/status`

## 2. このリポジトリの進め方

1. [`workshop/claude-code-hands-on-30min.md`](workshop/claude-code-hands-on-30min.md) を上から順番に読む
2. 画面のコマンドはその場でコピペして実行
3. ここには補助資料は置きません。必要コマンドは全部本編内にあります

## 3. Skill / Subagent について

- Skill: `.claude/skills/<name>/SKILL.md` の形で保存し、再利用できる作業手順を管理
- Subagent: `.claude/agents/<name>.md` で役割ごとの作業ルールを分離

## 4. 構成

- [`workshop/claude-code-hands-on-30min.md`](workshop/claude-code-hands-on-30min.md): 本編（30分）
- [`workshop/streamlit-claude-lab/data/sample.csv`](workshop/streamlit-claude-lab/data/sample.csv): ハンズオン用データ（1,000行）
- `.github/workflows/`（紹介のみ）:
  - [claude-code-review.yml](/Users/ryutoyoda/claude_code_learn/.github/workflows/claude-code-review.yml)
  - [claude-pr-assistant.yml](/Users/ryutoyoda/claude_code_learn/.github/workflows/claude-pr-assistant.yml)
- `.claude/skills/commit-helper/SKILL.md`: 追加例
- `.claude/agents/*.md`: サンプルSubagent

## 5. 参考リンク

- https://code.claude.com/docs/ja/overview
- https://code.claude.com/docs/ja/setup
- https://code.claude.com/docs/ja/skills
- https://code.claude.com/en/docs/claude-code/subagents
- https://github.com/streamlit/agent-skills
