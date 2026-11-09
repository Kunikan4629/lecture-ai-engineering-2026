# 第2回 解答

## 演習

- 使ったモデル・設定（`result.json` の `config` から）：  
  "config": {  
     "base_model": "Qwen/Qwen3.5-0.8B",  
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",  
    "seed": 42,  
    "num_train_epochs": 20,  
    "warmup_steps": 6,  
    "lr": 0.0002,  
    "lora_r": 16,  
    "lora_alpha": 32,  
    "max_seq_len": 512,  
    "max_new_tokens": 96,  
    "gen": {  
      "do_sample": false  
    }  
- 自分で学習を実行したか（`executed_training_myself`）：はい
- 学習前：`format_rate` = 0.095238 / `content_rate` =　0.000000
- 学習後：`format_rate` =　1.000000 / `content_rate` =　0.904762
- 退行した質問：なし
- 学習後に残った失敗と、その型（根拠なしの質問に断定した / 根拠ありの質問に確認要求で逃げた / 形式の崩れ）：
  - 失敗した質問１：「成績はいつ出ますか」/ 根拠なしの質問に断定した
  - 失敗した質問２：「オフィスアワーは事前予約が必要ですか」/ 根拠なしの質問に断定した
- 知識を問う質問（セクション 6）の結果（学習前の分類 → 学習後の分類）と気づいたこと：
  - 結果：学習前は全て「format_error」だが、学習後は「asserted」が2件、「needs_review」が2件存在
  - 気づいたこと：締切や期限といった、「課題の提出期限」という話から日時の情報を取得し、FAQにない「講座の最終締め切り」にも回答し、ハルシネーションを起こしていた
- 学習で変わったこと・変わらなかったこと：
  - 変わったこと：回答形式が指定のものに揃う割合が上がった
  - 変わらなかったこと：学習できていない知識に対しては対応できず、ハルシネーションも起こしてしまう

## 宿題

- 取り組んだレベル：基礎
- 変えた設定やデータ：エポック数を20回から1回に減らした
- 結果（比較表や数字）：
  - サマリ：エポック数を減らしたことで、format_rate（形式遵守率）は100%から90%に低下し、content_rate(内容遵守率)は90%から67%に低下した。 
  - result_json（変更前）：  
{
  "config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 20,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  },
  "versions": {
    "transformers": "5.18.0",
    "peft": "0.21.1",
    "trl": "1.14.2",
    "torch": "2.14.1+cu130"
  },
  "method": "LoRA（量子化なし）",
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 52.2
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.9047619047619048,
    "both_rate": 0.9047619047619048,
    "seconds": 43.3
  },
  "regressed": [],
  "knowledge": [
    {
      "質問": "この講座の最終課題の締切はいつですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"9 月 30 日 23:59\"}",
      "学習後の分類": "asserted",
      "学習後の出力": "{\"answer\": \"課題の提出期限は 9 月 30 日 23:59（日本時間）です。\", \"needs_review\": false}"
    },
    {
      "質問": "担当教員の名前は何ですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"確認が必要\"}",
      "学習後の分類": "needs_review",
      "学習後の出力": "{\"answer\": \"この FAQ には記載がありません。運営に確認が必要です。\", \"needs_review\": true}"
    },
    {
      "質問": "次回の講義のテーマは何ですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"次回の講義のテーマは「学習ポータル」の「課題」欄で提出する PDF 1 ファイルを提出します。\"}",
      "学習後の分類": "needs_review",
      "学習後の出力": "{\"answer\": \"この FAQ には記載がありません。運営に確認が必要です。\", \"needs_review\": true}"
    },
    {
      "質問": "課題の提出期限はいつですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"9 月 30 日 23:59\"}",
      "学習後の分類": "asserted",
      "学習後の出力": "{\"answer\": \"課題の提出期限は 9 月 30 日 23:59（日本時間）です。\", \"needs_review\": false}"
    }
  ],
  "executed_training_myself": true
}
  - result_json（変更後）：
{
  "config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 1,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  },
  "versions": {
    "transformers": "5.18.0",
    "peft": "0.21.1",
    "trl": "1.14.2",
    "torch": "2.14.1+cu130"
  },
  "method": "LoRA（量子化なし）",
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 40.9
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 0.9047619047619048,
    "content_rate": 0.6666666666666666,
    "both_rate": 0.6666666666666666,
    "seconds": 31.2
  },
  "regressed": [],
  "executed_training_myself": true
}
- ファインチューニングが向く場面・向かない場面についての考察：出力形式を調整したい場合はファインチューニングが有効だと感じた。特に今回の宿題の実験では、エポック数が20回の場合は形式を遵守する割合が100%だったのに対して、エポック数を1回にしても90%の性能を誇ったので、計算コストとの兼ね合いによっては少ない学習量でも十分期待する形式は得られることがわかった。
