# nais platform-build-push-sign action

An action that builds, pushes and signs a docker image.
Used primarily by nais-features.
By default images get a version of the format: `{{date 'YYYY-MM-DD-HHmmss' tz='Europe/Oslo'}}-{{sha}}` and another version `latest`.
If you need additional versions you can, optionally, use the extra_tags field.

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

The action produce two outputs, `tag` and `version`.
They can be retrieved with `${{ steps.build-push-sign.outputs.tag|version }}`.

`tag` is the whole Docker image name (ex. `europe-north1-docker.pkg.dev/nais-io/nais/images/smsmanager:2023-09-20-160128-1ededf3`), while `version` is just the part after the colon (ex. `2023-09-20-160128-1ededf3`).
