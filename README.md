# do-not-track-cli

A comprehensive `.env` file that opts out of telemetry, analytics, and data
collection across 100+ CLI tools, frameworks, SDKs, and runtimes — in one
place.

## Why

[donottrack.sh](https://donottrack.sh/) promotes a single universal variable
(`DO_NOT_TRACK=1`), but most tools haven't adopted it. Every framework invents
its own opt-out variable, buried in its docs.

[toptout](https://github.com/beatcracker/toptout) has the most complete data
set but outputs complex conditional shell scripts that require PowerShell or
careful sourcing.

This project takes a different approach: a single, flat `.env` file you can
drop anywhere, source once, and forget.

> "Collecting all the 'do not track' env vars into a single 'do_not_track.env'
> file... may not be a bad idea" — [PufPufPuf](https://news.ycombinator.com/item?id=47989846),
> in response to a [Hacker News discussion](https://news.ycombinator.com/item?id=47988592)
> about [donottrack.sh](https://donottrack.sh/)

The `DO_NOT_TRACK` standard itself was created by
[sneak (Jeffrey Paul)](https://github.com/sneak/consoledonottrack.com) at
[consoledonottrack.com](https://consoledonottrack.com/).

## Quick Start

```sh
# Clone or download
git clone https://github.com/alloydwhitlock/do-not-track-cli.git

# Source it in your current shell session
set -a && source do_not_track.env && set +a
```

Add to your shell profile to apply permanently:

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
    echo "Sourced $( grep -c '=' do_not_track.env ) opt-out variables"
```

### 12-factor apps / dotenv

Any tool that loads `.env` files (dotenv, direnv, docker, compose) can consume
`do_not_track.env` directly.

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

## Contributing

Pull requests welcome. To add a tool:

1. Find the official documentation for its telemetry opt-out variable.
2. Add an entry to `do_not_track.env` in the appropriate section:
   ```
   # Tool Name
   TOOL_TELEMETRY_DISABLED=1
   ```
3. Include the tool name as a comment and the correct opt-out value.
4. Open a PR with the tool name and a link to the official docs in the
   description.

**Notes:**
- Use the actual opt-out value from official docs (not just `1` if the tool
  expects `true` or `false`).
- If a tool already respects `DO_NOT_TRACK=1` (set at the top of the file),
  note it with a comment rather than adding a redundant entry.
- Generic variables like `CI=1` are intentionally excluded — they alter
  unrelated tool behavior.

## Credits

- [sneak (Jeffrey Paul)](https://github.com/sneak) — created the
  [DO_NOT_TRACK / Console Do Not Track](https://consoledonottrack.com/) standard
- [beatcracker/toptout](https://github.com/beatcracker/toptout) — the most
  comprehensive data source for tool-specific opt-out variables; many entries
  here are sourced from that project
- [PufPufPuf](https://news.ycombinator.com/item?id=47989846) — suggested the
  `.env` file format approach on Hacker News

## License

MIT
