# nais platform-build-push-sign action

An action that builds, pushes and signs a docker image. Used primarily by nais-features.
By default images get a tag on the format: `{{date 'YYYYMMDD-hhmmss' tz='Europe/Oslo'}}-{{sha}}` and another tag `latest`.
If you need additional tags you can, optionally, use the extra_tags field.

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
      - uses: actions/checkout@ac593985615ec2ede58e132d2e21d2b1cbd6127c # ratchet:actions/checkout@v3
      - uses: nais/platform-build-push-sign@main
        with:
          name: deployd
          dockerfile: Dockerfile
          google_service_account: gh-deploy
          push: ${{ github.actor != 'dependabot[bot]' }}
          workload_identity_provider: ${{ secrets.NAIS_IO_WORKLOAD_IDENTITY_PROVIDER }}
```
