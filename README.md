# term

Ask Google Gemini a question from any terminal, and get a streamed answer back.

```console
$ term what is the capital of France
The capital of France is Paris.
```

`term` is a single Python script with no required dependencies. It only uses the standard library.

## Features

- **One command, no quotes needed:** `term how do I undo the last git commit`
- **Streaming output:** the answer prints as it arrives.
- **Pipe input:** send a file or command output in as context.
- **Undercover mode (`-c`):** dresses the answer up as `pip install` output, progress bar included.

## Requirements

- Python 3.6+
- A Gemini API key (free from [Google AI Studio](https://aistudio.google.com/apikey))
- Optional: [`certifi`](https://pypi.org/project/certifi/), for Python builds that ship without CA certificates. `term` falls back to the system certificates if it isn't installed.

## Installation

```sh
git clone https://github.com/walkmoody/term.git
cd term
cp .env.example .env
```

Open `.env` and paste in your API key:

```sh
GEMINI_API_KEY=your-key-here
```

Then put `term` on your `PATH`, for example with a symlink:

```sh
ln -s "$PWD/term" /usr/local/bin/term
```

`term` follows symlinks to find its own directory, so it will still read the `.env` file in the cloned repo.

## Usage

```sh
term <your question>
```

### Pipe in context

Anything piped to `term` is added after your question:

```sh
cat error.log | term what is going wrong here
git diff | term write a commit message for this
echo "some text" | term summarize this
```

### Undercover mode

`-c` (or `--code`) formats the response so it looks like a package install:

```console
$ term -c what is the capital of France
Collecting answer
  Using cached pythonifiy-py3-none-any.whl.metadata (2.1 kB)
  Downloading response
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 0.0 kB/0.0 kB 0.0 MB/s eta 0:00:00
Installing collected packages: answer
  The capital of France is Paris.
Successfully installed answer-1.0
```

In a real terminal the progress bar is colored and updates live. Markdown is stripped from the answer and the text is dimmed.

## Configuration

Settings are read from the `.env` file next to the script. Environment variables that are already set take precedence over it.

| Variable         | Required | Default            | Description                     |
| ---------------- | -------- | ------------------ | ------------------------------- |
| `GEMINI_API_KEY` | Yes      | none               | Your Gemini API key             |
| `GEMINI_MODEL`   | No       | `gemini-2.5-flash` | Any Gemini model that supports streaming |

You can also set them per command:

```sh
GEMINI_MODEL=gemini-2.5-pro term explain monads simply
```

## Troubleshooting

- **`GEMINI_API_KEY is not set`:** make sure `.env` exists in the same directory as the real `term` script (not the symlink) and contains your key.
- **SSL certificate errors on macOS:** run `pip3 install certifi`, or run the `Install Certificates.command` that ships with python.org installers.
- **`Gemini API error 400/403`:** check that your key is valid and that the model name in `GEMINI_MODEL` exists.

## Privacy

Your questions and anything you pipe in are sent to Google's Gemini API. Don't pipe in secrets.
