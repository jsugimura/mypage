#ChatGPT #codex #AI #プログラミング

[公式HP](https://chatgpt.com/ja-JP/overview/)
[# OpenAI Codexの使い方を初心者向けに解説！特徴・料金・はじめ方まとめ](https://global-axis.jp/blog/openai-codex/)
[# AIコーディングエージェント「Codex」とは？特長と使い方を実演で徹底解説](https://devlog.mescius.jp/ai-agent-codex-quickstart/)

- ChatGPTのAIエージェント。
- ローカルにインストールしてIDEやCLIを通じて使用することも、クラウド上で使用することも可能。
- ローカルにインストールした場合は、権限の設定が重要。適切に設定しないと、処理してほしくないデータやファイルを書き換えられたり、消去されたりする可能性があるので注意。
- GitHubとの連携機能あり。
- 使用量の制限がある（[公式HP](https://chatgpt.com/ja-JP/codex/pricing/)）。特に5時間ごとにメッセージやり取りに制限が設けられており、そのほかに週単位でもトークン上限がある。
	- 次のような場合は、トークン消費量（[トークンとは](https://help.openai.com/en/articles/4936856-what-are-tokens-and-how-to-count-them)）が多くなる可能性がある。
		- 長い会話を続ける
		- AGENTS.mdのファイルサイズが大きい
		- 大量の資料を読み込ませる
		- モデルが高度である
- 上限に近づいたら軽量モデル（GPT-5.4-miniなど）に切り替えると長く使える。
- いきなり本番のディレクトリ下で作業するよりも、まずはテストディレクトリ、ファイルを作って実行しましょう。

## インストール
- ~~[公式HP](https://chatgpt.com/ja-JP/overview/)からインストーラをダウンロード、ChatGPTをインストール。~~  これにはCodex CLIは入っていない。
- [公式HP](https://learn.chatgpt.com/docs/codex/cli#getting-started)でインストール方法を参照。
	```
	> curl -fsSL https://chatgpt.com/codex/install.sh | sh
	==> Installing Codex CLI
	==> Detected platform: macOS (Apple Silicon)
	==> Resolved version: 0.146.0
	==> Downloading Codex CLI
	==> Installing standalone package to /Users/sugimura/.codex/packages/standalone/releases/0.146.0-aarch64-apple-darwin
	==> /Users/sugimura/.local/bin is already on PATH
	==> Current terminal: codex
	==> Future terminals: open a new terminal and run: codex
	Codex CLI 0.146.0 installed successfully.
	Start Codex now? [y/N] y
	```
	```
	> codex
	> You are in /Users/sugimura/test/codex
	
	  Do you trust the contents of this directory? Working with untrusted contents comes with higher risk of prompt injection. Trusting the directory allows project-local config, hooks, and exec policies to load.
	
	› 1. Yes, continue
	  2. No, quit
	  
	  Press enter to continue
	```
	```
	╭───────────────────────────────────────────╮
	│ >_ OpenAI Codex (v0.146.0)                │
	│                                           │
	│ model:     gpt-5.6-sol   /model to change │
	│ directory: ~/test/codex                   │
	╰───────────────────────────────────────────╯
	
	  Tip: Try the **Desktop app**. Run 'codex app' or visit https://chatgpt.com/codex?app-landing-page=true
	
	› Run /review on my current changes
	  gpt-5.6-sol default · ~/test/codex
	```
- ログイン状況確認
	```
	› /status
		╭────────────────────────────────────────────────────────────╮
		│  >_ OpenAI Codex (v0.146.0)                                │
		│                                                            │
		│ Visit https://chatgpt.com/codex/settings/usage for up-to-date  │
		│ information on rate limits and credits                     │
		│                                                            │
		│  Model:                gpt-5.6-sol (reasoning low, summaries auto) │
		│  Directory:            ~/test/codex                        │
		│  Permissions:          Workspace (Ask for approval)        │
		│  Agents.md:            <none>                              │
		│  Account:              <mail address>         (Plus)       │
		│  Collaboration mode:   Default                             │
		│  Session:              019fac9d-ae89-7030-bc0f-40d80bc5bb6f│
		│                                                            │
		│  Weekly limit:         [████████████████████] 100% left (resets 15:41 on 5 Aug) │
		╰────────────────────────────────────────────────────────────╯
	```

## 設定
### config.toml
- `~/.codex/config.toml`に以下を追記。このファイルでは、Codexに与える技術的な権限を決める。
	```
	# ~/.codex/config.toml
	
	# 生成したコマンドについて、必要に応じて承認を求める
	approval_policy = "on-request"
	
	# 作業領域内のみ書き込み可能
	sandbox_mode = "workspace-write"
	
	# Codex自身によるWeb検索も無効化
	web_search = "disabled"
	
	# シェルコマンドからのネットワーク通信を無効化
	[sandbox_workspace_write]
	network_access = false
	
	# 子プロセスへ渡す環境変数を限定する
	[shell_environment_policy]
	include_only = [
	    "PATH",
	    "HOME",
	    "LANG",
	    "LC_ALL",
	    "TMPDIR"
	]
	```

### AGENTS.md
- グローバルな`~/.codex/AGENTS.md`とプロジェクトごとの`<project>/AGENTS.md`が作成可能。前者はどのプロジェクトでも読み込まれる。
- まずは、`~/.codex/AGENTS.md`を作成する。このファイルでは、Codexに守らせる作業方針・開発ルールを決める。この作成により実行時間やトークン消費量を抑えられることがある（[# AIコーディングエージェント「Codex」とは？特長と使い方を実演で徹底解説](https://devlog.mescius.jp/ai-agent-codex-quickstart/)）。
- まずの設定はこんな感じ（[[AGENTS]]）。

## 主な操作方法
- 開始、終了
	```
	> codex
	› /exit
	```
	- exit後にトークン消費量が表示される。
		```
		Token usage: total=87,157 input=69,990 (+ 1,113,088 cached) output=17,167 (reasoning 2,657)
		To continue this session, run codex resume 019fb09a-ce81-7dc1-a5b3-b33e96253609
		```
		- GPT-5.6 Solでは、100万トークンあたりで、
			- 入力トークン: 125 クレジット
			- キャッシュ入力トークン: 12.5 クレジット
			- 出力トークン: 750 クレジット
		- なので、
			`0.069990 x 125 + 1.113088 x 12.5 + 0.017167 x 750 = 35.5376 クレジット`
		- を消費したことになる。
		- Plusでは、週次制限の1%程度に相当した。
- 過去セッションの参照。
	```
	> codex resume
	(参照したい過去ログを選択する)
	```
- 現在のセッションのconfigと、トークン使用量の確認
	```
	› /status
	```
	- `Context window`とは、現在のセッションで保持できる情報量の目安。これが0%になっても利用停止となることはないが、古い情報を圧縮する必要が出て、以前の会話を参照しにくくなる可能性がある。
	- `/compact`によって、現在の会話を要約し、重要事項を残して`context window`の容量を確保することが可能。