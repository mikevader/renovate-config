# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) presets.

## Presets

| Preset | Description |
|---|---|
| `mikevader/renovate-config` | Base config (`default.json`) |
| `mikevader/renovate-config:renovate-annotations` | Regex manager for `# renovate:` annotations in YAML files |

### `renovate-annotations`

Updates versions in any `.yaml`/`.yml` file that have a `# renovate:` comment on the line directly above a `tag:`, `appVersion:` or `version:` key.

```json5
{
  extends: [
    'mikevader/renovate-config',
    'mikevader/renovate-config:renovate-annotations',
  ],
}
```

Two annotation forms are supported (`[...]` is optional):

```yaml
# Docker image
# renovate: image=ghcr.io/mikevader/plex [versioning=loose]
appVersion: 1.41.0

# Any other datasource
# renovate: datasource=github-releases [depName=foo] packageName=owner/repo [versioning=semver]
version: 1.2.3
```

- `datasource` and `versioning` default to `docker`.
- Values may be quoted (`tag: "v1.2.3"`), and the key may be indented.
- If the repository config sets `enabledManagers`, add `custom.regex` to it.
- Files excluded by the repository's `ignorePaths` are skipped.

## Validation

`.github/workflows/validate.yaml` runs `renovate-config-validator --strict` with the latest Renovate on every push, on pull requests and weekly. Renovate does not open config migration PRs for presets, so a failing run means a preset is invalid or needs migrating; the log contains the diff to apply.
