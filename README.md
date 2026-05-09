# Codex Switch Provider

This is a simple script to switch the provider of the Codex CLI, and sync the chat history among different providers.

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

Download `codex-switch-provider` and move it to your PATH, for example:
```bash
chmod +x codex-switch-provider
mv codex-switch-provider ~/.local/bin/
```

### 4. Switch the provider

```bash
codex-switch-provider 0011ai
```

or change to original provider

```bash
codex-switch-provider openai
```

It will backup the config.toml, state_5.sqlite, and session files, and switch to the new provider, and sync all the chat history to the new provider.

### 5. Restore to the last backup

If you worry a switch caused data loss, you can restore everything to the most recent backup snapshot:

```bash
codex-switch-provider restore
```

This restores:
- `config.toml`
- `state_5.sqlite` (and `-wal`/`-shm` if present)
- backed-up session files from the same backup timestamp

### 6. List available backup snapshots

```bash
codex-switch-provider list-backups
```

This prints backup timestamps and whether each snapshot includes `config`, `state`, and `session`.