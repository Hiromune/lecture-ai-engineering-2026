# 第6回 解答

## 演習

<!-- [tool call] の行は、表示された行をそのまま貼り付けてください。終わらなかった STEP は、空欄のままでかまいません。 -->

- STEP 1 のツールの名前：従来のエージェントは `search_materials`、MCPエージェントは `glossary_search_glossary`。
- STEP 2 の [tool call]：

  ```text
  [tool call]   glossary_search_glossary({'query': '3レベル'})
  ```

- STEP 3 の [tool call]：

  ```text
  [tool call]   fetch_fetch({'url': 'https://modelcontextprotocol.io/'})
  ```

- STEP 4 の [tool call]：

  ```text
  [tool call]   aws_aws___search_documentation({'search_phrase': 'Amazon S3 バージョニング 概要', 'topics': ['general']})
  [tool call]   aws_aws___search_documentation({'search_phrase': 'Using versioning Amazon S3', 'topics': ['general']})
  ```

- 気づき・疑問：用語集とfetchはCodespaces内でコマンド起動し、AWSのサーバーにはURLで接続した。fetchが未登録のときも、モデルはWebページを要約したような回答を返したが、実際には用語集しか参照していなかった。回答文だけでなく、`--trace`で実際のツール呼び出しを確認することが重要だと分かった。

## 宿題

<!-- 基礎・実装・発展のどれか1つに取り組めば完了です。 -->

- 取り組んだレベル：基礎
- やったこと：サーバー側の `app/mcp_server.py` のTODOの下に `server.tool()(tools.get_lecture_summary)` を1行追加した。演習分をコミットした後、`git status` と `git diff` で、宿題の変更がこのファイルの1行だけであることを確認した。宿題ではエージェント本体と接続設定を変更していない。
- 結果：ツール一覧が7個から8個に増え、`glossary_get_lecture_summary` が表示された。「第2回では何を学びますか？」という質問に対し、`[tool call]   glossary_get_lecture_summary({'lecture': 2})` が記録され、第2回のファインチューニングとLoRAについて回答された。`uv run pytest -q` の結果は `21 passed in 4.54s` で、すべてのテストが成功した。
- 気づき・疑問：サーバー側でツールを公開すれば、エージェント側を変更しなくても、新しいツールが一覧に現れ、質問に応じて使われることを確認できた。ツールの登録は一覧で、実際の利用は `--trace` で確認できる。コマンドの案内と提出文の整理にAIを利用し、自分のCodespacesで実行した結果を記録した。
