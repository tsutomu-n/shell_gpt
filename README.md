## ShellGPT シェルGPT

A command-line productivity tool powered by AI large language models (LLM). This command-line tool offers streamlined generation of **shell commands, code snippets, documentation**, eliminating the need for external resources (like Google search). Supports Linux, macOS, Windows and compatible with all major Shells like PowerShell, CMD, Bash, Zsh, etc.  
AI大規模言語モデル（LLM）によって強化されたコマンドライン生産性ツール。このコマンドラインツールは、シェルコマンド、コードスニペット、ドキュメントの効率的な生成を提供し、外部リソース（Google検索など）の必要性を排除します。Linux、macOS、Windowsをサポートし、PowerShell、CMD、Bash、Zshなどのすべての主要なシェルと互換性があります。

ShellGPT.mp4

## Installation インストール

By default, ShellGPT uses OpenAI's API and GPT-4 model. You'll need an API key, you can generate one [here](https://beta.openai.com/account/api-keys). You will be prompted for your key which will then be stored in `~/.config/shell_gpt/.sgptrc`. OpenAI API is not free of charge, please refer to the [OpenAI pricing](https://openai.com/pricing) for more information.  
デフォルトでは、ShellGPTはOpenAIのAPIとGPT-4モデルを使用します。APIキーが必要ですが、こちらで生成できます。キーが求められ、それが `~/.config/shell_gpt/.sgptrc` に保存されます。OpenAI APIは無料ではありませんので、詳細はOpenAIの価格設定を参照してください。

Tip チップ

Alternatively, you can use locally hosted open source models which are available for free. To use local models, you will need to run your own LLM backend server such as [Ollama](https://github.com/ollama/ollama). To set up ShellGPT with Ollama, please follow this comprehensive [guide](https://github.com/TheR1D/shell_gpt/wiki/Ollama).  
代わりに、無料で利用可能なローカルにホストされたオープンソースモデルを使用することもできます。ローカルモデルを使用するには、OllamaなどのLLMバックエンドサーバーを実行する必要があります。OllamaとShellGPTをセットアップするには、この包括的なガイドに従ってください。

**❗️Note that ShellGPT is not optimized for local models and may not work as expected.  
❗️ShellGPTはローカルモデルに最適化されていないため、期待通りに動作しない場合があります。**

## Usage 使用

**ShellGPT** is designed to quickly analyse and retrieve information. It's useful for straightforward requests ranging from technical configurations to general knowledge.  
ShellGPTは情報を迅速に分析して取得するように設計されています。技術的な構成から一般的な知識まで、簡単なリクエストに役立ちます。

```shell
sgpt "What is the fibonacci sequence"
# -> The Fibonacci sequence is a series of numbers where each number ...
```

ShellGPT accepts prompt from both stdin and command line argument. Whether you prefer piping input through the terminal or specifying it directly as arguments, `sgpt` got you covered. For example, you can easily generate a git commit message based on a diff:  
ShellGPTは、標準入力とコマンドライン引数の両方からプロンプトを受け入れます。入力をターミナルを介してパイプ処理するか、引数として直接指定するかにかかわらず、 `sgpt` が対応しています。例えば、差分を基にgitのコミットメッセージを簡単に生成できます。

```shell
git diff | sgpt "Generate git commit message, for my changes"
# -> Added main feature details into README.md
```

You can analyze logs from various sources by passing them using stdin, along with a prompt. For instance, we can use it to quickly analyze logs, identify errors and get suggestions for possible solutions:  
さまざまなソースからのログをstdinを使用して渡すことで、プロンプトと一緒に解析することができます。たとえば、ログを迅速に分析し、エラーを特定し、可能な解決策の提案を得るために使用できます。

```shell
docker logs -n 20 my_app | sgpt "check logs, find errors, provide possible solutions"
```

```
Error Detected: Connection timeout at line 7.
Possible Solution: Check network connectivity and firewall settings.
Error Detected: Memory allocation failed at line 12.
Possible Solution: Consider increasing memory allocation or optimizing application memory usage.
```

You can also use all kind of redirection operators to pass input:  
入力を渡すために、すべての種類のリダイレクション演算子を使用することもできます。

```shell
sgpt "summarise" < document.txt
# -> The document discusses the impact...
sgpt << EOF
What is the best way to lear Golang?
Provide simple hello world example.
EOF
# -> The best way to learn Golang...
sgpt <<< "What is the best way to learn shell redirects?"
# -> The best way to learn shell redirects is through...
```

### Shell commands シェルコマンド

Have you ever found yourself forgetting common shell commands, such as `find`, and needing to look up the syntax online? With `--shell` or shortcut `-s` option, you can quickly generate and execute the commands you need right in the terminal.  
あなたは、 `find` などの一般的なシェルコマンドを忘れてしまい、オンラインで構文を調べる必要があることがありますか？ `--shell` またはショートカット `-s` オプションを使用すると、必要なコマンドをすばやく生成して実行できます。

```shell
sgpt --shell "find all json files in current folder"
# -> find . -type f -name "*.json"
# -> [E]xecute, [D]escribe, [A]bort: e
```

Shell GPT is aware of OS and `$SHELL` you are using, it will provide shell command for specific system you have. For instance, if you ask `sgpt` to update your system, it will return a command based on your OS. Here's an example using macOS:  
Shell GPTは、使用しているOSと `$SHELL` を認識しており、お持ちの特定のシステム用のシェルコマンドを提供します。たとえば、 `sgpt` にシステムを更新するように依頼すると、OSに基づいたコマンドが返されます。macOSを使用した例を以下に示します：

```shell
sgpt -s "update my system"
# -> sudo softwareupdate -i -a
# -> [E]xecute, [D]escribe, [A]bort: e
```

The same prompt, when used on Ubuntu, will generate a different suggestion:  
同じプロンプトをUbuntuで使用すると、異なる提案が生成されます。

```shell
sgpt -s "update my system"
# -> sudo apt update && sudo apt upgrade -y
# -> [E]xecute, [D]escribe, [A]bort: e
```

Let's try it with Docker:  
Dockerで試してみましょう。

```shell
sgpt -s "start nginx container, mount ./index.html"
# -> docker run -d -p 80:80 -v $(pwd)/index.html:/usr/share/nginx/html/index.html nginx
# -> [E]xecute, [D]escribe, [A]bort: e
```

We can still use pipes to pass input to `sgpt` and generate shell commands:  
私たちはまだパイプを使用して `sgpt` に入力を渡し、シェルコマンドを生成することができます。

```shell
sgpt -s "POST localhost with" < data.json
# -> curl -X POST -H "Content-Type: application/json" -d '{"a": 1, "b": 2}' http://localhost
# -> [E]xecute, [D]escribe, [A]bort: e
```

Applying additional shell magic in our prompt, in this example passing file names to `ffmpeg`:  
私たちのプロンプトに追加のシェルマジックを適用すると、この例ではファイル名を `ffmpeg` に渡します：

```shell
ls
# -> 1.mp4 2.mp4 3.mp4
sgpt -s "ffmpeg combine $(ls -m) into one video file without audio."
# -> ffmpeg -i 1.mp4 -i 2.mp4 -i 3.mp4 -filter_complex "[0:v] [1:v] [2:v] concat=n=3:v=1 [v]" -map "[v]" out.mp4
# -> [E]xecute, [D]escribe, [A]bort: e
```

If you would like to pass generated shell command using pipe, you can use `--no-interaction` option. This will disable interactive mode and will print generated command to stdout. In this example we are using `pbcopy` to copy generated command to clipboard:  
生成されたシェルコマンドをパイプを使って渡したい場合は、 `--no-interaction` オプションを使用できます。これにより対話モードが無効になり、生成されたコマンドが標準出力に出力されます。この例では、生成されたコマンドをクリップボードにコピーするために `pbcopy` を使用しています。

```shell
sgpt -s "find all json files in current folder" --no-interaction | pbcopy
```

### Shell integration シェル統合

This is a **very handy feature**, which allows you to use `sgpt` shell completions directly in your terminal, without the need to type `sgpt` with prompt and arguments. Shell integration enables the use of ShellGPT with hotkeys in your terminal, supported by both Bash and ZSH shells. This feature puts `sgpt` completions directly into terminal buffer (input line), allowing for immediate editing of suggested commands.  
これは非常に便利な機能で、プロンプトや引数を入力する必要なく、 `sgpt` のシェル補完をターミナルで直接使用できるようにします。シェル統合により、ShellGPTをホットキーで使用できるようになり、BashとZSHの両方でサポートされます。この機能により、 `sgpt` の補完がターミナルバッファ（入力行）に直接挿入され、提案されたコマンドをすぐに編集できます。

Shell\_GPT\_Integration.mp4

To install shell integration, run `sgpt --install-integration` and restart your terminal to apply changes. This will add few lines to your `.bashrc` or `.zshrc` file. After that, you can use `Ctrl+l` (by default) to invoke ShellGPT. When you press `Ctrl+l` it will replace you current input line (buffer) with suggested command. You can then edit it and just press `Enter` to execute.  
シェル統合をインストールするには、 `sgpt --install-integration` を実行し、変更を適用するためにターミナルを再起動してください。これにより、 `.bashrc` または `.zshrc` ファイルにいくつかの行が追加されます。その後、ShellGPTを呼び出すために `Ctrl+l` （デフォルト）を使用できます。 `Ctrl+l` を押すと、提案されたコマンドで現在の入力行（バッファ）が置き換えられます。その後、それを編集して `Enter` を押すだけで実行できます。

### Generating code コードを生成します。

By using the `--code` or `-c` parameter, you can specifically request pure code output, for instance:  
`--code` または `-c` パラメーターを使用することで、純粋なコード出力を要求することができます。例えば：

```shell
sgpt --code "solve fizz buzz problem using python"
```

```python
for i in range(1, 101):
    if i % 3 == 0 and i % 5 == 0:
        print("FizzBuzz")
    elif i % 3 == 0:
        print("Fizz")
    elif i % 5 == 0:
        print("Buzz")
    else:
        print(i)
```

Since it is valid python code, we can redirect the output to a file:  
有効なPythonコードであるため、出力をファイルにリダイレクトできます。

```shell
sgpt --code "solve classic fizz buzz problem using Python" > fizz_buzz.py
python fizz_buzz.py
# 1
# 2
# Fizz
# 4
# Buzz
# ...
```

We can also use pipes to pass input:  
入力を渡すためにパイプも使用できます。

```shell
cat fizz_buzz.py | sgpt --code "Generate comments for each line of my code"
```

```python
# Loop through numbers 1 to 100
for i in range(1, 101):
    # Check if number is divisible by both 3 and 5
    if i % 3 == 0 and i % 5 == 0:
        # Print "FizzBuzz" if number is divisible by both 3 and 5
        print("FizzBuzz")
    # Check if number is divisible by 3
    elif i % 3 == 0:
        # Print "Fizz" if number is divisible by 3
        print("Fizz")
    # Check if number is divisible by 5
    elif i % 5 == 0:
        # Print "Buzz" if number is divisible by 5
        print("Buzz")
    # If number is not divisible by 3 or 5, print the number itself
    else:
        print(i)
```

### Chat Mode チャットモード

Often it is important to preserve and recall a conversation. `sgpt` creates conversational dialogue with each LLM completion requested. The dialogue can develop one-by-one (chat mode) or interactively, in a REPL loop (REPL mode). Both ways rely on the same underlying object, called a chat session. The session is located at the [configurable](https://github.com/TheR1D/shell_gpt#runtime-configuration-file) `CHAT_CACHE_PATH`.  
しばしば会話を保存して呼び出すことが重要です。 `sgpt` は、リクエストされたLLM完了ごとに会話のダイアログを作成します。ダイアログは1つずつ（チャットモード）または対話的に、REPLループ（REPLモード）で展開できます。両方の方法は、チャットセッションと呼ばれる同じ基礎オブジェクトに依存しています。セッションは設定可能な `CHAT_CACHE_PATH` に配置されています。

To start a conversation, use the `--chat` option followed by a unique session name and a prompt.  
会話を開始するには、一意のセッション名とプロンプトに続いて `--chat` オプションを使用してください。

```shell
sgpt --chat conversation_1 "please remember my favorite number: 4"
# -> I will remember that your favorite number is 4.
sgpt --chat conversation_1 "what would be my favorite number + 4?"
# -> Your favorite number is 4, so if we add 4 to it, the result would be 8.
```

You can use chat sessions to iteratively improve GPT suggestions by providing additional details. It is possible to use `--code` or `--shell` options to initiate `--chat`:  
追加の詳細情報を提供することで、チャットセッションを使用してGPTの提案を反復的に改善することができます。 `--code` または `--shell` オプションを使用して `--chat` を開始することが可能です。

```shell
sgpt --chat conversation_2 --code "make a request to localhost using python"
```

```python
import requests

response = requests.get('http://localhost')
print(response.text)
```

Let's ask LLM to add caching to our request:  
私たちのリクエストにキャッシュを追加するようにLLMに依頼しましょう。

```shell
sgpt --chat conversation_2 --code "add caching"
```

```python
import requests
from cachecontrol import CacheControl

sess = requests.session()
cached_sess = CacheControl(sess)

response = cached_sess.get('http://localhost')
print(response.text)
```

Same applies for shell commands:  
同じことがシェルコマンドにも適用されます。

```shell
sgpt --chat conversation_3 --shell "what is in current folder"
# -> ls
sgpt --chat conversation_3 "Sort by name"
# -> ls | sort
sgpt --chat conversation_3 "Concatenate them using FFMPEG"
# -> ffmpeg -i "concat:$(ls | sort | tr '\n' '|')" -codec copy output.mp4
sgpt --chat conversation_3 "Convert the resulting file into an MP3"
# -> ffmpeg -i output.mp4 -vn -acodec libmp3lame -ac 2 -ab 160k -ar 48000 final_output.mp3
```

To list all the sessions from either conversational mode, use the `--list-chats` or `-lc` option:  
どちらかの会話モードからすべてのセッションをリストするには、 `--list-chats` または `-lc` オプションを使用してください。

```shell
sgpt --list-chats
# .../shell_gpt/chat_cache/conversation_1  
# .../shell_gpt/chat_cache/conversation_2
```

To show all the messages related to a specific conversation, use the `--show-chat` option followed by the session name:  
特定の会話に関連するすべてのメッセージを表示するには、セッション名の後に `--show-chat` オプションを使用してください：

```shell
sgpt --show-chat conversation_1
# user: please remember my favorite number: 4
# assistant: I will remember that your favorite number is 4.
# user: what would be my favorite number + 4?
# assistant: Your favorite number is 4, so if we add 4 to it, the result would be 8.
```

### REPL Mode REPLモード

There is very handy REPL (read–eval–print loop) mode, which allows you to interactively chat with GPT models. To start a chat session in REPL mode, use the `--repl` option followed by a unique session name. You can also use "temp" as a session name to start a temporary REPL session. Note that `--chat` and `--repl` are using same underlying object, so you can use `--chat` to start a chat session and then pick it up with `--repl` to continue the conversation in REPL mode.  
非常に便利な REPL（read–eval–print loop）モードがあり、GPT モデルと対話的にチャットすることができます。 REPL モードでチャットセッションを開始するには、一意のセッション名に続いて `--repl` オプションを使用します。一時的な REPL セッションを開始するには、セッション名として "temp" を使用することもできます。 `--chat` と `--repl` は同じ基礎オブジェクトを使用しているため、 `--chat` を使用してチャットセッションを開始し、その後 `--repl` で会話を続けることができます。

[![gif](https://camo.githubusercontent.com/9efc234cbf1d582faa5ed2581c8fa0fb60de615be418ac2f3afb5de5a8e4bf5b/68747470733a2f2f7331302e67696679752e636f6d2f696d616765732f7265706c2d64656d6f2e676966)](https://camo.githubusercontent.com/9efc234cbf1d582faa5ed2581c8fa0fb60de615be418ac2f3afb5de5a8e4bf5b/68747470733a2f2f7331302e67696679752e636f6d2f696d616765732f7265706c2d64656d6f2e676966)

```
sgpt --repl temp
Entering REPL mode, press Ctrl+C to exit.
&gt;&gt;&gt; What is REPL?
REPL stands for Read-Eval-Print Loop. It is a programming environment ...
&gt;&gt;&gt; How can I use Python with REPL?
To use Python with REPL, you can simply open a terminal or command prompt ...
```

REPL mode can work with `--shell` and `--code` options, which makes it very handy for interactive shell commands and code generation:  
REPLモードは `--shell` と `--code` のオプションと連携して動作し、対話型シェルコマンドやコード生成に非常に便利です。

```
sgpt --repl temp --shell
Entering shell REPL mode, type [e] to execute commands or press Ctrl+C to exit.
&gt;&gt;&gt; What is in current folder?
ls
&gt;&gt;&gt; Show file sizes
ls -lh
&gt;&gt;&gt; Sort them by file sizes
ls -lhS
&gt;&gt;&gt; e (enter just e to execute commands, or d to describe them)
```

To provide multiline prompt use triple quotes `"""`:  
複数行のプロンプトを提供するには、トリプルクォートを使用してください `"""` ：

```
sgpt --repl temp
Entering REPL mode, press Ctrl+C to exit.
&gt;&gt;&gt; """
... Explain following code:
... import random
... print(random.randint(1, 10))
... """
It is a Python script that uses the random module to generate and print a random integer.
```

You can also enter REPL mode with initial prompt by passing it as an argument or stdin or even both:  
初期プロンプトを引数または標準入力、またはその両方として渡すことで、REPLモードに入ることもできます。

```shell
sgpt --repl temp < my_app.py
```

```
Entering REPL mode, press Ctrl+C to exit.
──────────────────────────────────── Input ────────────────────────────────────
name = input("What is your name?")
print(f"Hello {name}")
───────────────────────────────────────────────────────────────────────────────
&gt;&gt;&gt; What is this code about?
The snippet of code you've provided is written in Python. It prompts the user...
&gt;&gt;&gt; Follow up questions...
```

### Function calling 機能の呼び出し

[Function calls](https://platform.openai.com/docs/guides/function-calling) is a powerful feature OpenAI provides. It allows LLM to execute functions in your system, which can be used to accomplish a variety of tasks. To install [default functions](https://github.com/TheR1D/shell_gpt/tree/main/sgpt/default_functions/) run:  
機能呼び出しは、OpenAIが提供する強力な機能です。これによりLLMは、システム内の関数を実行でき、さまざまなタスクを達成するために使用できます。デフォルトの機能をインストールするには、次のコマンドを実行します：

ShellGPT has a convenient way to define functions and use them. In order to create your custom function, navigate to `~/.config/shell_gpt/functions` and create a new .py file with the function name. Inside this file, you can define your function using the following syntax:  
ShellGPTには、関数を定義して使用する便利な方法があります。カスタム関数を作成するには、 `~/.config/shell_gpt/functions` に移動して、関数名を持つ新しい .py ファイルを作成します。このファイル内で、次の構文を使用して関数を定義できます：

```python
# execute_shell_command.py
import subprocess
from pydantic import Field
from instructor import OpenAISchema


class Function(OpenAISchema):
    """
    Executes a shell command and returns the output (result).
    """
    shell_command: str = Field(..., example="ls -la", descriptions="Shell command to execute.")

    class Config:
        title = "execute_shell_command"

    @classmethod
    def execute(cls, shell_command: str) -> str:
        result = subprocess.run(shell_command.split(), capture_output=True, text=True)
        return f"Exit code: {result.returncode}, Output:\n{result.stdout}"
```

The docstring comment inside the class will be passed to OpenAI API as a description for the function, along with the `title` attribute and parameters descriptions. The `execute` function will be called if LLM decides to use your function. In this case we are allowing LLM to execute any Shell commands in our system. Since we are returning the output of the command, LLM will be able to analyze it and decide if it is a good fit for the prompt.  
クラス内のdocstringコメントは、 `title` 属性とパラメータの説明と共に、関数の説明としてOpenAI APIに渡されます。LLMがあなたの関数を使用することを決定した場合、 `execute` 関数が呼び出されます。この場合、LLMにはシステム内の任意のシェルコマンドを実行することが許可されています。コマンドの出力を返すため、LLMはそれを分析し、プロンプトに適しているかどうかを判断できます。  
Here is an example how the function might be executed by LLM:  
こちらは、機能がLLMによって実行される方法の例です：

```shell
sgpt "What are the files in /tmp folder?"
# -> @FunctionCall execute_shell_command(shell_command="ls /tmp")
# -> The /tmp folder contains the following files and directories:
# -> test.txt
# -> test.json
```

Note that if for some reason the function (execute\_shell\_command) will return an error, LLM might try to accomplish the task based on the output. Let's say we don't have installed `jq` in our system, and we ask LLM to parse JSON file:  
関数（execute\_shell\_command）が何らかの理由でエラーを返す場合、LLMは出力に基づいてタスクを達成しようとするかもしれません。システムに `jq` がインストールされていないとしましょう。LLMにJSONファイルを解析するように依頼します。

```shell
sgpt "parse /tmp/test.json file using jq and return only email value"
# -> @FunctionCall execute_shell_command(shell_command="jq -r '.email' /tmp/test.json")
# -> It appears that jq is not installed on the system. Let me try to install it using brew.
# -> @FunctionCall execute_shell_command(shell_command="brew install jq")
# -> jq has been successfully installed. Let me try to parse the file again.
# -> @FunctionCall execute_shell_command(shell_command="jq -r '.email' /tmp/test.json")
# -> The email value in /tmp/test.json is johndoe@example.
```

It is also possible to chain multiple function calls in the prompt:  
プロンプトで複数の関数呼び出しを連鎖させることも可能です。

```shell
sgpt "Play music and open hacker news"
# -> @FunctionCall play_music()
# -> @FunctionCall open_url(url="https://news.ycombinator.com")
# -> Music is now playing, and Hacker News has been opened in your browser. Enjoy!
```

This is just a simple example of how you can use function calls. It is truly a powerful feature that can be used to accomplish a variety of complex tasks. We have dedicated [category](https://github.com/TheR1D/shell_gpt/discussions/categories/functions) in GitHub Discussions for sharing and discussing functions. LLM might execute destructive commands, so please use it at your own risk❗️  
これは、関数呼び出しをどのように使用できるかの単純な例です。さまざまな複雑なタスクを達成するために使用できる本当に強力な機能です。GitHub Discussionsには、関数の共有や議論のための専用カテゴリがあります。LLMは破壊的なコマンドを実行する可能性があるため、自己責任で使用してください❗️

### Roles 役割

ShellGPT allows you to create custom roles, which can be utilized to generate code, shell commands, or to fulfill your specific needs. To create a new role, use the `--create-role` option followed by the role name. You will be prompted to provide a description for the role, along with other details. This will create a JSON file in `~/.config/shell_gpt/roles` with the role name. Inside this directory, you can also edit default `sgpt` roles, such as **shell**, **code**, and **default**. Use the `--list-roles` option to list all available roles, and the `--show-role` option to display the details of a specific role. Here's an example of a custom role:  
ShellGPTを使用すると、コード、シェルコマンドを生成したり、特定のニーズを満たすために使用できるカスタムロールを作成できます。新しいロールを作成するには、ロール名に続いて `--create-role` オプションを使用します。ロールの説明やその他の詳細を提供するよう求められます。これにより、ロール名を持つJSONファイルが `~/.config/shell_gpt/roles` に作成されます。このディレクトリ内では、シェル、コード、デフォルトなどのデフォルトの `sgpt` ロールも編集できます。すべての利用可能なロールをリストするには `--list-roles` オプションを使用し、特定のロールの詳細を表示するには `--show-role` オプションを使用します。以下はカスタムロールの例です：

```shell
sgpt --create-role json_generator
# Enter role description: Provide only valid json as response.
sgpt --role json_generator "random: user, password, email, address"
```

```json
{
  "user": "JohnDoe",
  "password": "p@ssw0rd",
  "email": "johndoe@example.com",
  "address": {
    "street": "123 Main St",
    "city": "Anytown",
    "state": "CA",
    "zip": "12345"
  }
}
```

If the description of the role contains the words "APPLY MARKDOWN" (case sensitive), then chats will be displayed using markdown formatting.  
役割の説明に「APPLY MARKDOWN」という単語が含まれている場合（大文字と小文字を区別する）、チャットはmarkdown形式で表示されます。

### Request cache キャッシュをリクエスト

Control cache using `--cache` (default) and `--no-cache` options. This caching applies for all `sgpt` requests to OpenAI API:  
`--cache` （デフォルト）および `--no-cache` オプションを使用してキャッシュを制御します。このキャッシングはOpenAI APIへのすべての `sgpt` リクエストに適用されます。

```shell
sgpt "what are the colors of a rainbow"
# -> The colors of a rainbow are red, orange, yellow, green, blue, indigo, and violet.
```

Next time, same exact query will get results from local cache instantly. Note that `sgpt "what are the colors of a rainbow" --temperature 0.5` will make a new request, since we didn't provide `--temperature` (same applies to `--top-probability`) on previous request.  
次回、同じクエリーはローカルキャッシュから即座に結果を取得します。 `sgpt "what are the colors of a rainbow" --temperature 0.5` は新しいリクエストを作成しますので、前回のリクエストで `--temperature` ( `--top-probability` にも同じことが適用されます) を提供していないことに注意してください。

This is just some examples of what we can do using OpenAI GPT models, I'm sure you will find it useful for your specific use cases.  
これは、OpenAI GPT モデルを使用して行うことができるいくつかの例です。きっとあなたの特定のユースケースに役立つと思います。

### Runtime configuration file  
ランタイム構成ファイル

You can setup some parameters in runtime configuration file `~/.config/shell_gpt/.sgptrc`:  
実行時設定ファイル `~/.config/shell_gpt/.sgptrc` でいくつかのパラメータを設定できます：

```
# API key, also it is possible to define OPENAI_API_KEY env.
OPENAI_API_KEY=your_api_key
# Base URL of the backend server. If "default" URL will be resolved based on --model.
API_BASE_URL=default
# Max amount of cached message per chat session.
CHAT_CACHE_LENGTH=100
# Chat cache folder.
CHAT_CACHE_PATH=/tmp/shell_gpt/chat_cache
# Request cache length (amount).
CACHE_LENGTH=100
# Request cache folder.
CACHE_PATH=/tmp/shell_gpt/cache
# Request timeout in seconds.
REQUEST_TIMEOUT=60
# Default OpenAI model to use.
DEFAULT_MODEL=gpt-3.5-turbo
# Default color for shell and code completions.
DEFAULT_COLOR=magenta
# When in --shell mode, default to "Y" for no input.
DEFAULT_EXECUTE_SHELL_CMD=false
# Disable streaming of responses
DISABLE_STREAMING=false
# The pygment theme to view markdown (default/describe role).
CODE_THEME=default
# Path to a directory with functions.
OPENAI_FUNCTIONS_PATH=/Users/user/.config/shell_gpt/functions
# Print output of functions when LLM uses them.
SHOW_FUNCTIONS_OUTPUT=false
# Allows LLM to use functions.
OPENAI_USE_FUNCTIONS=true
# Enforce LiteLLM usage (for local LLMs).
USE_LITELLM=false
```

Possible options for `DEFAULT_COLOR`: black, red, green, yellow, blue, magenta, cyan, white, bright\_black, bright\_red, bright\_green, bright\_yellow, bright\_blue, bright\_magenta, bright\_cyan, bright\_white. Possible options for `CODE_THEME`: [https://pygments.org/styles/](https://pygments.org/styles/)  
`DEFAULT_COLOR` の可能なオプション：黒、赤、緑、黄、青、マゼンタ、シアン、白、明るい黒、明るい赤、明るい緑、明るい黄、明るい青、明るいマゼンタ、明るいシアン、明るい白。 `CODE_THEME` の可能なオプション：https://pygments.org/styles/

### Full list of arguments 引数の完全なリスト

```
╭─ Arguments ──────────────────────────────────────────────────────────────────────────────────────────────╮
│   prompt      [PROMPT]  The prompt to generate completions for.                                          │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭─ Options ────────────────────────────────────────────────────────────────────────────────────────────────╮
│ --model            TEXT                       Large language model to use. [default: gpt-4-1106-preview] │
│ --temperature      FLOAT RANGE [0.0&lt;=x&lt;=2.0]  Randomness of generated output. [default: 0.0]             │
│ --top-p            FLOAT RANGE [0.0&lt;=x&lt;=1.0]  Limits highest probable tokens (words). [default: 1.0]     │
│ --md             --no-md                      Prettify markdown output. [default: md]                    │
│ --editor                                      Open $EDITOR to provide a prompt. [default: no-editor]     │
│ --cache                                       Cache completion results. [default: cache]                 │
│ --version                                     Show version.                                              │
│ --help                                        Show this message and exit.                                │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭─ Assistance Options ─────────────────────────────────────────────────────────────────────────────────────╮
│ --shell           -s                      Generate and execute shell commands.                           │
│ --interaction         --no-interaction    Interactive mode for --shell option. [default: interaction]    │
│ --describe-shell  -d                      Describe a shell command.                                      │
│ --code            -c                      Generate only code.                                            │
│ --functions           --no-functions      Allow function calls. [default: functions]                     │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭─ Chat Options ───────────────────────────────────────────────────────────────────────────────────────────╮
│ --chat                 TEXT  Follow conversation with id, use "temp" for quick session. [default: None]  │
│ --repl                 TEXT  Start a REPL (Read–eval–print loop) session. [default: None]                │
│ --show-chat            TEXT  Show all messages from provided chat id. [default: None]                    │
│ --list-chats  -lc            List all existing chat ids.                                                 │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────╯
╭─ Role Options ───────────────────────────────────────────────────────────────────────────────────────────╮
│ --role                  TEXT  System role for GPT model. [default: None]                                 │
│ --create-role           TEXT  Create role. [default: None]                                               │
│ --show-role             TEXT  Show role. [default: None]                                                 │
│ --list-roles   -lr            List roles.                                                                │
╰──────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```

## Docker

Run the container using the `OPENAI_API_KEY` environment variable, and a docker volume to store cache:  
`OPENAI_API_KEY` 環境変数を使用してコンテナを実行し、キャッシュを保存するためのDockerボリュームを使用します。

```shell
docker run --rm \
           --env OPENAI_API_KEY="your OPENAI API key" \
           --volume gpt-cache:/tmp/shell_gpt \
       ghcr.io/ther1d/shell_gpt --chat rainbow "what are the colors of a rainbow"
```

Example of a conversation, using an alias and the `OPENAI_API_KEY` environment variable:  
エイリアスと `OPENAI_API_KEY` 環境変数を使用した会話の例：

```shell
alias sgpt="docker run --rm --env OPENAI_API_KEY --volume gpt-cache:/tmp/shell_gpt ghcr.io/ther1d/shell_gpt"
export OPENAI_API_KEY="your OPENAI API key"
sgpt --chat rainbow "what are the colors of a rainbow"
sgpt --chat rainbow "inverse the list of your last answer"
sgpt --chat rainbow "translate your last answer in french"
```

You also can use the provided `Dockerfile` to build your own image:  
あなたも提供された `Dockerfile` を使用して、独自のイメージを作成することができます。

Additional documentation: [Azure integration](https://github.com/TheR1D/shell_gpt/wiki/Azure), [Ollama integration](https://github.com/TheR1D/shell_gpt/wiki/Ollama).  
追加のドキュメント: Azure 統合、Ollama 統合。
