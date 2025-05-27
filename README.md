# 🧠 aider config

This repository contains configuration and setup instructions for running [Aider](https://github.com/Aider-AI/aider)
using models served locally by [LM Studio](https://lmstudio.ai/).

> ⚡️ Aider is a powerful AI pair programmer that works directly with your code in Git repositories.
> This setup enables it to leverage fast, local large language models (LLMs) through LM Studio for enhanced privacy and offline performance.

---

## 📦 Requirements

- [LM Studio](https://lmstudio.ai/) (Linux, macOS, or Windows)
- A compatible local LLM (e.g., `mistral`, `phi`, `llama`, etc.)
- Python 3.10+
- Git (for working in repositories)

---

## 🚀 LM Studio Installation

1. **Download LM Studio**  
   Go to [https://lmstudio.ai](https://lmstudio.ai) and download the app for your platform.

2. **Install and Launch**  
   Follow the installation instructions for your OS. Open LM Studio once installed.

3. **Download a Supported Model**  
   Inside LM Studio, go to the **Models** tab and download a supported model such as:

   - `deepseek-coder-v2-lite-instruct`
   - `qwen2.5-coder-32b-instruct`

4. **Enable Local Server Mode**
   - Navigate to the **Settings** tab (⚙️).
   - Enable the **"Enable Local Server"** option.
   - Make sure the server is bound to `localhost` or `0.0.0.0` (depending on your needs).
   - Note the server port (default: `1234`).

---

## 🐍 Installing Aider

```bash
python -m pip install --user aider-install
aider-install
```

yeah, that's it

---

## 🔧 Environment Configuration

Aider needs to know where to send requests to your local LM Studio server.
You can [configure this using environment variables](https://aider.chat/docs/llms/lm-studio.html).

> Even though LM Studio doesn’t require an API Key out of the box
> the `LM_STUDIO_API_KEY` must have a dummy value like `dummy-api-key` set
> or the client request will fail trying to send an empty Bearer token.

### For Bash / Zsh

Add the following to your `~/.bashrc`, `~/.zshrc`, or `~/.bash_profile`:

```bash
export LM_STUDIO_API_BASE=http://localhost:1234/v1
export LM_STUDIO_API_KEY=dummy-api-key
```

After editing, reload your shell config:

```bash
source ~/.bashrc   # or ~/.zshrc
```

### For Fish Shell

```fish
set -Ux LM_STUDIO_API_BASE http://localhost:1234/v1
set -Ux LM_STUDIO_API_KEY dummy-api-key
```

## ⚙️ Model Configuration

Here is where we get to the meat of this repository.
These configuration files are designed to be symlinked into your home directory:

| File                | Purpose                                                                 | Doc                                                                                                                               |
| ------------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| conf.yml            | Customizes Aider's behavior and toolchain integration.                  | [Aider Config](https://aider.chat/docs/config/aider_conf.html)                                                                    |
| model.metadata.json | Describes the selected model's properties for Aider compatibility.      | [Context window size and token costs](https://aider.chat/docs/config/adv-model-settings.html#context-window-size-and-token-costs) |
| model.settings.yml  | Model-specific instructions or behavioral tweaks for conversation flow. | [Model settings](https://aider.chat/docs/config/adv-model-settings.html#model-settings)                                           |

### For Bash / Zsh

```bash
ln -s "$(pwd)/conf.yml" ~/.aider.conf.yml

ln -s "$(pwd)/model.metadata.json" ~/.aider.model.metadata.json

ln -s "$(pwd)/model.settings.yml" ~/.aider.model.settings.yml
```

### For Fish Shell

```fish
ln -s (pwd)/conf.yml ~/.aider.conf.yml

ln -s (pwd)/model.metadata.json ~/.aider.model.metadata.json

ln -s (pwd)/model.settings.yml ~/.aider.model.settings.yml
```

## 🧪 Test Your Setup

Once the environment variables are set and LM Studio is running:

```bash
aider
```

You should see Aider attempt to connect to your locally running LLM.
