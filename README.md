# nais platform-build-push-sign action

An action that builds, pushes and signs a docker image.
Used primarily by nais-features.
By default images get a version of the format: `{{date 'YYYY-MM-DD-HHmmss' tz='Europe/Oslo'}}-{{sha}}` and another version `latest`.
If you need additional versions you can, optionally, use the extra_tags field.

The `image` output is the canonical image reference. When a digest is available, it is returned as `<registry>/<repo>:<tag>@<digest>` so deployments and attestation can use an immutable image reference while still preserving the readable tag.

If no pushed-image digest is available, the `image` output falls back to the tagged image reference only. This happens when `push` is `false` and `push_ghcr` is `false`.

When `push` or `push_ghcr` is `true`, the action requires a digest from the underlying build step before attestation/signing runs. The action also fails early if the configured build context directory or Dockerfile does not exist.

## Usage

```yaml
name: Build, push, and sign image
jobs:
  build_push_sign:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write
    steps:
      - uses: actions/checkout@v4
      - uses: nais/platform-build-push-sign@main
        id: build-push-sign
        with:
          name: deployd
          dockerfile: Dockerfile
          google_service_account: gh-deploy
          push: ${{ github.actor != 'dependabot[bot]' }}
          workload_identity_provider: ${{ secrets.NAIS_IO_WORKLOAD_IDENTITY_PROVIDER }}
```

## Output

The action produces four outputs: `tag`, `version`, `image`, and `digest`.
They can be retrieved with `${{ steps.build-push-sign.outputs.<name> }}`.

`tag` is the whole Docker image name (ex. `europe-north1-docker.pkg.dev/nais-io/nais/images/smsmanager:2023-09-20-160128-1ededf3`).

`version` is just the tag part (ex. `2023-09-20-160128-1ededf3`).

`image` is the canonical image reference. When available, it includes both the readable tag and immutable digest (ex. `europe-north1-docker.pkg.dev/nais-io/nais/images/smsmanager:2023-09-20-160128-1ededf3@sha256:abc123...`).

`digest` is the image digest from the build step (ex. `sha256:abc123...`).
