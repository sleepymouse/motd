# Versioning

This project uses [axion-release-plugin](https://github.com/allegro/axion-release-plugin) to derive the version from git tags. The JAR filename and container image tag are always in sync.

## How it works

The version is determined by the state of the git tree relative to the most recent tag:

- **Tagged commit** — the version is the tag exactly, with no suffix
- **Untagged commit** — the version is the next patch increment with a `-SNAPSHOT` suffix

The `SNAPSHOT` suffix indicates a development build that has not been formally released.

## Tagging convention

Tags must be prefixed with `v`:

```bash
git tag v1.2.3
```

Without the `v` prefix the plugin will not recognise the tag.

## Release workflow

To produce a release build:

1. Commit all changes as normal
2. Create a tag on that commit:
   ```bash
   git tag v0.1.0
   ```
3. Push the tag to trigger the build pipeline:
   ```bash
   git push origin --tags
   ```

The pipeline will produce:
- `motd-0.1.0.jar`
- `ghcr.io/sleepymouse/motd:0.1.0`

## Version progression examples

| Action | Resulting version |
|---|---|
| Tag `v0.4.0` and push | `0.4.0` |
| Commit to main (no tag) | `0.4.1-SNAPSHOT` |
| Tag `v0.4.1` and push | `0.4.1` |
| Commit to main (no tag) | `0.4.2-SNAPSHOT` |
| Tag `v0.5.0` and push | `0.5.0` |

## Controlling the version

The patch number is incremented automatically. The minor and major numbers are only changed by explicitly choosing a higher tag:

| Tag you create | Version produced |
|---|---|
| `v0.4.1` | `0.4.1` — patch release |
| `v0.5.0` | `0.5.0` — minor release |
| `v1.0.0` | `1.0.0` — major release |

You are always in control — nothing is bumped automatically beyond patch snapshots between releases.
