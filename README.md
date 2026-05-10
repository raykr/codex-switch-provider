# x-switch (Codex provider 切换)

主命令名为 **`x-switch`**，仓库内脚本文件名也是 `x-switch`。

用于切换 Codex CLI 的 provider，并在不同 provider 之间同步会话与本地状态。

## Usage

### 1. Setup environment variables

```bash
echo 'export OPENAI_API_KEY_0011AI="xxxx"' >> ~/.bashrc
echo 'export OPENAI_API_KEY_0011AI="xxxx"' >> ~/.pam_environment
source ~/.bashrc
source ~/.pam_environment
```

### 2. Add a new provider to `config.toml`

```toml
model_provider = "0011ai"

[model_providers.0011ai]
name = "0011ai"
base_url = "https://aicoding.2233.ai"
env_key = "OPENAI_API_KEY_0011AI"
wire_api = "responses"
requires_openai_auth = false

[profiles.openai]
model_provider = "openai"

[profiles.0011ai]
model_provider = "0011ai"
```

Note: `openai` is the built-in default provider in Codex, so you do not need to add a `[model_providers.openai]` section.

### 3. Switch the provider

Download `x-switch` and move it to your PATH, for example:
```bash
chmod +x x-switch
mv x-switch ~/.local/bin/
```

### 4. Switch the provider

```bash
x-switch 0011ai
```

or change to original provider

```bash
x-switch openai
```

It will backup the config.toml, state_5.sqlite, and session files, and switch to the new provider, and sync all the chat history to the new provider.

### 5. Restore from backup

If you worry a switch caused data loss, you can restore everything to the most recent backup snapshot:

```bash
x-switch restore
```

Or restore from a specific timestamp (from `list-backups` output):

```bash
x-switch restore 20260509-165001
```

This restores:
- `config.toml`
- `state_5.sqlite` (and `-wal`/`-shm` if present)
- backed-up session files from the same backup timestamp

### 6. List available backup snapshots

```bash
x-switch list-backups
```

This prints backup timestamps and whether each snapshot includes `config`, `state`, and `session`.