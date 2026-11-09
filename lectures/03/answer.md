# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：
    - 第1回の回答（知識なし）：AIE2026（AIエンジニア育成講座2026年版など）の宿題における「3レベル」についてですね。一般的に、AIやプログラミング系の実践講座（AIEなど）で出される宿題の「3レベル制」は、受講生のスキルや理解度に合わせて**「初級・中級・上級」**や**「必須・推奨・発展」**のように難易度が3段階に分かれている課題を指します。
    - 今回の回答（知識あり）：「3レベル」とは、AIE2026の宿題の構成のことです。宿題には以下の3つの段階（レベル）が用意されており、受講者はどれか1つに取り組めばその回の宿題完了となります。原則として採点は行われません。1. **基礎**：講義内容の確認 2. **実装**：自分でコードを書く 3. **発展**：調査や試行
- `--trace` で見えたツール呼び出し（ツール名と引数）：[tool call]   search_glossary({'query': '3レベル'})
[tool return] search_glossary -> [{"term": "宿題の3レベル", "lecture": 1, "definition": "AIE2026の宿題の構成。「基礎（講義内容の確認）」「実装（自分でコードを書く）」「発展（調査や試行）」の3段階があり、どれか1つに取り組…
- 1つの質問での LLM 呼び出し回数（`requests`）：2回（ツール呼び出しと合わせた回数）
- 追加したツールと、docstring に書いた説明：make_quizツール「指定した回の用語から 1つを選び、確認問題と答えを返す。「第2回の確認問題を出して」のように、復習用の問題を求められたときに使う。」

## 宿題

- 取り組んだレベル：発展（b）
- やったこと：コーディングエージェントのPiを動かし、Piのシステムプロンプトと自分のエージェントのINSTRUCTIONSを比較する
- 結果：
    - Piのシステムプロンプト：You are a coding assistant running inside pi, a coding agent harness. Help users inspect files, run commands, and modify code. 
<tools>
read: Read file contents
bash: Execute bash commands (ls, grep, find, etc.)
edit: Make precise file edits with exact text replacement, including multiple disjoint edits in one call
write: Create or overwrite files
Additional tools may be available.
</tools> 
<rules>
Use bash for file operations like ls, rg, find
Use read to examine files instead of cat or sed.
You can inspect PI_* environment variables for current model and session details.
Use edit for targeted changes. Batch changes to one file in a single call.
Each edits[].oldText must exactly and uniquely match the original file. Keep matches short and non-overlapping; merge nearby changes without including large unchanged regions.
Use write only for new files or complete rewrites.
Be concise in your responses
Show file paths clearly when working with files
</rules> 
<docs>
Pi documentation (read only when the user asks about the harness itself, its SDK, extensions, themes, skills, or TUI):
Main documentation: /usr/local/share/nvm/versions/node/v26.13.0/lib/node_modules/@earendil-works/pi-coding-agent/README.md
Additional docs: /usr/local/share/nvm/versions/node/v26.13.0/lib/node_modules/@earendil-works/pi-coding-agent/docs
Examples: /usr/local/share/nvm/versions/node/v26.13.0/lib/node_modules/@earendil-works/pi-coding-agent/examples (extensions, custom tools, SDK)
When reading pi docs or examples, resolve docs/... under Additional docs and examples/... under Examples, not the current working directory
When asked about: extensions (docs/extensions.md, examples/extensions/), themes (docs/themes.md), skills (docs/skills.md), prompt templates (docs/prompt-templates.md), TUI components (docs/tui.md), keybindings (docs/keybindings.md), SDK integrations (docs/sdk.md), custom providers (docs/custom-provider.md), adding models (docs/models.md), packages (docs/packages.md), environment variables (docs/environment-variables.md), MCP servers (docs/mcp.md), codemode scripts and non-LLM models such as classifiers and image models (docs/codemode.md)
Before implementing harness changes, read the relevant docs and examples in full, following related cross-references.
</docs> 
<cwd>
/workspaces/lecture-ai-engineering-2026
</cwd>  
    - 自分のINSTRUCTIONS：あなたはAIエンジニアリング講座（AIE2026）の学習支援エージェントです。用語や講座に固有のことを答える前に、必ず search_glossary で用語集を確認する。回の内容を聞かれたら get_lecture_summary を使う。用語集に見つからないことは、推測で断定せず「用語集にはない」と伝える。回答の最後に、関係する回（第N回）を示す。宿題の答えそのものは書かず、考え方のヒントで導く。
- 気づき・疑問：汎用のpiはツールの使い方の作法しか書かずに判断をモデルに任せているため、手探りで5回以上APIを読んでいたが、用途に合わせて「どの場面でどのツールを使うか」まで指定した専用エージェントなら1回で済むので、ドメインに合ったツールと指示の有用性を検証できた。
