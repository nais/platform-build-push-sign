# nais platform-build-push-sign action

An action that builds, pushes and signs a docker image. Used primarilly by nais-features

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
      - uses: nais/build-push-sign@main
        with:
          name: deployd
          dockerfile: Dockerfile
          google_service_account: gh-deploy
          push: ${{ github.actor != 'dependabot[bot]' }}
          workload_identity_provider: ${{ secrets.NAIS_IO_WORKLOAD_IDENTITY_PROVIDER }}
```
