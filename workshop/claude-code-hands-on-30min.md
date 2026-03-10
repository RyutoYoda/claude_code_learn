# Claude Code ハンズオン

テーマ：**ClaudeCodeを使ってデータ可視化アプリを作る**


## ゴール
- Claude Code を起動できる
- `/help`, `/compact`, `/skills`, `/agents`, `/status` を使える
- `streamlit` 用のSkillを実際に自分で登録する
- `app.py` を作って可視化仕様を自然言語で確認できる
- 同梱データ `workshop/streamlit-claude-lab/data/sample.csv` は、1,000行の架空受注データ（`order_date`, `region`, `product`, `orders`, `sales`）
- 必要な環境は会場側で既に整えた前提で進めるため、アプリ実行に必要な追加コマンドは不要

## Step 1: インストールと起動
環境に合わせてインストールします。

### macOS / Linux / WSL

```bash
brew install --cask claude-code
```
### curl
```curl
curl -fsSL https://claude.ai/install.sh | bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
```
### Windows

```powershell
irm https://claude.ai/install.ps1 | iex
```

または

```powershell
winget install Anthropic.ClaudeCode
```

すでにインストール済みなら、このインストールブロックは飛ばしてOK。

```text
claude
```

初回のみ ` /login` が必要。起動後、次を入力欄へそのまま入力。

```text
/help
/help compact
/skills
/agents
/status
/compact このセッションの進め方を短く要約
```

### 各コマンドの役割

| コマンド | 何ができる？ |
|---|---|
| `/help` | Claude Code の使い方・コマンド一覧を表示。困ったらまずこれ |
| `/compact` | 会話履歴を要約して圧縮する。コンテキストが長くなったらリセット代わりに使う。引数にメッセージを渡すと、要約しつつ次の指示も出せる |
| `/skills` | 登録済みのスキル（専門知識テンプレート）を一覧表示。スキルがあると Claude の出力品質が上がる |
| `/agents` | カスタムエージェント（`.claude/agents/` 配下）を一覧表示。特定タスク専用の指示セットを呼び出せる |
| `/status` | 現在のセッション情報（モデル、権限モード、コンテキスト使用量など）を確認 |
| `/security-review` | 現在のブランチの差分をセキュリティ観点でレビュー。SQLインジェクションやXSSなどの脆弱性を自動チェックしてくれる |

### （例）`/status` の画面構成

#### Usage（使用量タブ）

上が今のセッションのAPIトークン使用量（日次リセット）
下が今週の累計使用量（全モデル合計・週次リセット）

<img width="526" height="223" alt="スクリーンショット 2026-03-09 0 49 26" src="https://github.com/user-attachments/assets/9ac1518d-e78c-41e8-a366-e2b72a550404" />

#### Config（設定タブ）

| 設定項目 | 値 | 説明 |
|---|---|---|
| Auto-compact | true | コンテキストが長くなったとき自動で圧縮する |
| Show tips | true | 操作のヒントをUI上に表示する |
| Reduce motion | false | アニメーションを減らすアクセシビリティ設定 |
| Thinking mode | true | Claude が思考過程を表示するモード |
| Rewind code (checkpoints) | true | コード変更前にチェックポイントを保存し巻き戻せる |
| Verbose output | false | 詳細なログ出力を有効にする |
| Terminal progress bar | true | 処理中にプログレスバーを表示する |
| Default permission mode | Default | ファイル操作などの許可モード（Default / Auto approve など） |
| Respect .gitignore in file picker | true | ファイル選択時に .gitignore を考慮する |
| Always copy full response | false | /copy 時に全レスポンスを自動コピーする |
| Auto-update channel | latest | Claude Code の自動更新チャンネル |
| Theme | Dark mode (colorblind-friendly) | UI テーマ（色覚特性に配慮したダークモード） |
| Notifications | Auto | 通知の表示タイミング |
| Output style | default | 出力フォーマットのスタイル |
| Language | Default (English) | 表示言語 |
| Editor mode | normal | エディタの操作モード |
| Show PR status footer | true | PR のステータスをフッターに表示する |
| Model | Default (recommended) | 使用する Claude モデル |
| Auto-install IDE extension | true | IDE 拡張機能を自動インストールする |
| Claude in Chrome enabled by default | true | Chrome 拡張のデフォルト有効化 |
| Enable Remote Control for all sessions | default | 全セッションでリモート操作を許可するか |

## Step 2: Skill を確認（読み込み）

このハンズオンで使う `developing-with-streamlit` は、まずこの画面で反映させます。  
起動後、以下を順番に打ってください。

```text
/skills
```

次に、Skill 配下に追加してください。

```bash
git clone https://github.com/streamlit/agent-skills.git /tmp/agent-skills                                                  
cp -r /tmp/agent-skills/developing-with-streamlit ./.claude/skills/ 
```
https://github.com/streamlit/agent-skills/tree/main/developing-with-streamlit

再度 `/skills` で反映を確認してください。

## Step 3: 生成内容の受け取り

ここであなたが打つのは、以降の `developing-with-streamlit` 指示だけです。  
Claude が返す実装内容を確認し、追加修正は Step5 へ進みます。

## Step 4: Claudeで最初のアプリを作らせる

`workshop/streamlit-claude-lab` の `data/sample.csv` を使って、以下を依頼します。

```text
developing-with-streamlit の技能で、次を作ってください。
- app.py を作成
- data/sample.csv を使った月次売上の集計可視化
- ラインチャートと棒グラフを表示
- 期間フィルタ（start_date と end_date）を作成
- データ表を表示
- 依存関係を使って requirements.txt も作成
- 実行できる最小構成にしてください
```

## Step 5: 改善を追加で依頼する

```text
自由に作ってください。好みの改善を1つだけ入れてください。

まず3つの改善案を出してから、最後に1つだけ実装してください。
```
## 参照
https://code.claude.com/docs/ja/overview
