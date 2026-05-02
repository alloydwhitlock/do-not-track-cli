# do-not-track-cli

A single `.env` file to opt out of telemetry across CLI tools, frameworks, SDKs, and runtimes.

## Why

[donottrack.sh](https://donottrack.sh/) promotes a universal `DO_NOT_TRACK=1` variable, but most tools haven't adopted it. Each framework defines its own opt-out variable, usually buried in its docs.

[toptout](https://github.com/beatcracker/toptout) maintains the most complete dataset but generates complex shell scripts that require PowerShell or careful conditional sourcing.

This project maintains a single, flat `.env` file. Source it in your shell profile, load it in `docker-compose.yml`, or use it with direnv — no scripting required.

## Quick Start

```sh
git clone https://github.com/alloydwhitlock/do-not-track-cli.git
set -a && source do_not_track.env && set +a
```

To persist across sessions, add to your shell profile:

```sh
# ~/.zshrc or ~/.bashrc
set -a && source /path/to/do_not_track.env && set +a
```

## Usage

### Shell (bash / zsh)

```sh
set -a && source do_not_track.env && set +a
```

### direnv

```sh
# In your project's .envrc
dotenv /path/to/do_not_track.env
```

Or copy the contents directly into `.envrc`.

### Docker Compose

```yaml
services:
  app:
    env_file:
      - do_not_track.env
```

### CI/CD (GitHub Actions)

```yaml
- name: Disable telemetry
  run: |
    set -a && source do_not_track.env && set +a
    echo "Sourced $( grep -cE '^[A-Z_][A-Z0-9_]*=' do_not_track.env ) opt-out variables"
```

### 12-factor apps / dotenv

Any tool that loads `.env` files (dotenv, direnv, Docker, Compose) can consume `do_not_track.env` directly.

## What's covered

| Category | Examples |
|---|---|
| JavaScript / TypeScript | Next.js, Nuxt, Astro, Gatsby, Angular, Turborepo, Strapi |
| Package managers | Homebrew, Yarn 2 |
| Language runtimes | .NET, Go, PowerShell Core, Bun |
| Infrastructure as Code | Terraform, Packer, Consul, Vault, Pulumi, Earthly, Pants |
| Cloud CLIs | gcloud, az, aws sam, gh (GitHub), sf (Salesforce), wrangler |
| Databases | InfluxDB, MeiliSearch, Hasura, NocoDB, Meltano |
| AI / ML | Gemini CLI, Hugging Face Hub |
| DevOps tools | Serverless Framework, Fastlane, CocoaPods, Chef, Arduino CLI |
| Other | Stripe CLI, Rover (Apollo), Expo, Rasa, Vagrant, vstest |

Full list: see [`do_not_track.env`](./do_not_track.env).

### Aggressive opt-ins

Some variables do more than disable telemetry — they block network access or use generic names that may conflict with other tools. These are in a commented-out section at the bottom of `do_not_track.env`. Uncomment the ones you want after verifying they won't break other tools in your environment.

| Variable | Tool | Side effect |
|---|---|---|
| `HF_HUB_OFFLINE=1` | Hugging Face Hub | Blocks all connections to hf.co, including model and dataset downloads |
| `ANALYTICS=no` | AccessMap | Generic name; may affect other tools that check `$ANALYTICS` |
| `TELEMETRY_ENABLED=0` | projector-cli | Generic name; may affect other tools that check `$TELEMETRY_ENABLED` |

## Contributing

Pull requests welcome. To add a tool:

1. Find the official documentation for its telemetry opt-out variable.
2. Add an entry to `do_not_track.env` in the appropriate section:
   ```
   # Tool Name
   TOOL_TELEMETRY_DISABLED=1
   ```
3. Include the tool name as a comment and the correct opt-out value.
4. Open a PR with the tool name and a link to the official docs in the description.

**Notes:**
- Use the actual opt-out value from official docs (not just `1` if the tool expects `true` or `false`).
- If a tool already respects `DO_NOT_TRACK=1` (set at the top of the file), note it with a comment rather than adding a redundant entry.
- Generic variables like `CI=1` are excluded — they alter unrelated tool behavior. Variables that affect more than telemetry belong in the aggressive opt-ins section at the bottom of the file.

## Credits

- [sneak (Jeffrey Paul)](https://github.com/sneak) — created the [DO_NOT_TRACK / Console Do Not Track](https://consoledonottrack.com/) standard
- [beatcracker/toptout](https://github.com/beatcracker/toptout) — the most comprehensive data source for tool-specific opt-out variables; many entries here are sourced from that project
- [PufPufPuf](https://news.ycombinator.com/item?id=47989846) — suggested the `.env` file format approach on Hacker News

## License

MIT
