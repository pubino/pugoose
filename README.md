# pugoose

Configure [Goose](https://github.com/block/goose) to work with [Portkey](https://portkey.ai) AI Gateway for access to multiple LLM providers through a single API.

## Prerequisites

- [Goose](https://github.com/block/goose) installed
- Portkey API key (e.g., from Princeton AI Sandbox or your own Portkey account)

## Installation

### 1. Copy the custom provider configuration

```bash
# Create the custom providers directory if it doesn't exist
mkdir -p ~/.config/goose/custom_providers

# Copy the Portkey provider config
cp config/custom_portkey.json ~/.config/goose/custom_providers/
```

### 2. Set your API key

Add to your `~/.zshrc` or `~/.bashrc`:

```bash
export CUSTOM_PORTKEY_API_KEY="your-portkey-api-key"
```

Then reload:

```bash
source ~/.zshrc
```

### 3. Configure Goose to use Portkey

Copy the example config or manually edit `~/.config/goose/config.yaml`:

```bash
cp config/config.yaml ~/.config/goose/config.yaml
```

Or create/edit manually:

```yaml
GOOSE_PROVIDER: custom_portkey
GOOSE_MODEL: gpt-4o-mini
```

## Usage

Start Goose:

```bash
goose
```

### Working with Local Repositories

To use Goose with files in a local repository, navigate to your project directory first:

```bash
cd /path/to/your/project
goose
```

Goose will have access to files in the current working directory. You can then ask it to:

- Read and analyze code files
- Make edits to existing files
- Create new files
- Run shell commands
- Execute tests

Example prompts:
- "Read the main.py file and explain what it does"
- "Find all TODO comments in this project"
- "Add error handling to the database connection function"
- "Run the test suite and fix any failures"

### Starting a Named Session

To resume work later, start a named session:

```bash
goose session start --name my-project
```

Resume later with:

```bash
goose session resume my-project
```

## Available Models

| Provider   | Models                                              |
|------------|-----------------------------------------------------|
| OpenAI     | gpt-5, gpt-4o, gpt-4o-mini, gpt-4-turbo, o3-mini   |
| Anthropic  | claude-sonnet-4-20250514, claude-3-5-sonnet-20241022 |
| Google     | gemini-2.5-pro, gemini-2.5-flash, gemini-2.0-flash |
| Mistral    | mistral-small-2503, Mistral-Large-2411             |
| Meta       | Llama-3.3-70B-Instruct, Meta-Llama-3-1-8B-Instruct |

## Switching Models

Edit `~/.config/goose/config.yaml`:

```yaml
GOOSE_MODEL: gemini-2.0-flash
```

## Cost-Effective Recommendations

For daily coding tasks, use these budget-friendly models:

| Model              | Input/1M tokens | Output/1M tokens |
|--------------------|-----------------|------------------|
| mistral-small-2503 | $0.10           | $0.30            |
| gpt-4o-mini        | $0.165          | $0.66            |

## Configuration Details

### Key Configuration Note

For Goose custom providers, the `base_url` must include the **full endpoint path**:

```
https://api.portkey.ai/v1/chat/completions
```

This differs from tools like Aider which only need `https://api.portkey.ai/v1`.

### Environment Variable

The API key is read from `CUSTOM_PORTKEY_API_KEY` environment variable.

## Troubleshooting

### 404 Resource Not Found

If you see this error, check:

1. `base_url` includes `/chat/completions` suffix
2. Model name doesn't have `openai/` prefix (use `gpt-4o-mini` not `openai/gpt-4o-mini`)
3. `CUSTOM_PORTKEY_API_KEY` environment variable is set

### Verify API Key

```bash
echo $CUSTOM_PORTKEY_API_KEY
```

### Test API Directly

```bash
curl -X POST "https://api.portkey.ai/v1/chat/completions" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $CUSTOM_PORTKEY_API_KEY" \
  -d '{"model": "gpt-4o-mini", "messages": [{"role": "user", "content": "hi"}]}'
```
