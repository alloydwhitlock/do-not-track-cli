# do-not-track-cli

A single `.env` file to opt out of telemetry across CLI tools, frameworks, SDKs, and runtimes.

## TL;DR

Clone the repo and source the file:

```sh
git clone https://github.com/alloydwhitlock/do-not-track-cli.git
set -a && source do_not_track.env && set +a
```

To load it in every shell session, add that line to `~/.zshrc` or `~/.bashrc`.

## Why

Most CLI tools collect telemetry by default and implement their own opt-out variables. The [donottrack.sh](https://donottrack.sh/) standard (`DO_NOT_TRACK=1`) is a good baseline, but adoption is uneven across the ecosystem. The [toptout project](https://github.com/beatcracker/toptout) catalogs per-tool opt-out variables; this project packages them as a plain `.env` file compatible with bash, direnv, Docker Compose, and any dotenv loader.

## Usage

### Shell (bash / zsh)

```sh
set -a && source do_not_track.env && set +a
```

### direnv

Copy the contents into your `.envrc`, or reference the file directly:

```sh
dotenv /path/to/do_not_track.env
```

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
    echo "Sourced $( grep -cE '^[A-Z_]+=' do_not_track.env ) opt-out variables"
```

### dotenv / 12-factor

Works with any tool that loads `.env` files: dotenv, direnv, Docker Compose, and similar.

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

Full list: [`do_not_track.env`](./do_not_track.env).

### Aggressive opt-ins

A few variables go further than disabling telemetry. They're in a commented-out section at the bottom of the file; read the comments before enabling them.

| Variable | Tool | Side effect |
|---|---|---|
| `HF_HUB_OFFLINE=1` | Hugging Face Hub | Blocks all connections to hf.co, model downloads included |
| `ANALYTICS=no` | AccessMap | Generic name that may affect other tools checking `$ANALYTICS` |
| `TELEMETRY_ENABLED=0` | projector-cli | Common enough to collide with other tools |

## Contributing

To add a tool, open a PR with:

- The entry added to the appropriate section of `do_not_track.env`:
  ```
  # Tool Name
  TOOL_TELEMETRY_DISABLED=1
  ```
- A link to the official docs confirming the variable name and value

A few things to check first:

- Use the exact value from the docs. If the tool expects `true`, don't substitute `1`.
- If the tool already respects `DO_NOT_TRACK=1`, add a comment noting that rather than a new entry.
- If the variable name is generic (like `ANALYTICS` or `TELEMETRY_ENABLED`), it belongs in the aggressive opt-ins section at the bottom of the file.

## Credits

[sneak (Jeffrey Paul)](https://github.com/sneak) created the [DO_NOT_TRACK / Console Do Not Track](https://consoledonottrack.com/) standard that many tools now support.

[beatcracker/toptout](https://github.com/beatcracker/toptout) maintains the most complete dataset of per-tool telemetry opt-out variables. Many entries here are sourced from that project.

[PufPufPuf](https://news.ycombinator.com/item?id=47989846) suggested the `.env` file format approach in a Hacker News discussion.

## License

MIT
