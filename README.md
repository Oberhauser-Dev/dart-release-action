# dart-release-action

A GitHub action of [dart_release](https://github.com/Oberhauser-Dev/dart_packages/tree/main/packages/dart_release) to build and deploy Dart apps for macOS, Linux and Windows.
The action itself runs on Linux, Windows, and macOS.
For a full example you can check out the workflow of [wrestling_scoreboard](https://github.com/Oberhauser-Dev/wrestling_scoreboard/blob/main/.github/workflows/release-server.yml).

## Usage

### Upload to a GitHub release

```yaml
steps:
- uses: actions/checkout@v4
- uses: dart-lang/setup-dart@v1
- uses: oberhauser-dev/dart-release-action@v0
  with:
    token: ${{ github.token }}
    dry-run: true
    app-name: 'my_dart_app'
    app-version: ${{ github.ref_name }} # or set manually: 'v1.2.3-alpha.4'
    tag: ${{ github.ref }}
    build-type: 'debian'
    build-args: |-
      --dart-define=API_URL="https://example.com"
      --dart-define=API_KEY=12345678
```

### Publish

Store your `Repository secrets` here: `https://github.com/<username>/<repository>/settings/secrets/actions`.

```yaml
steps:
- uses: actions/checkout@v4
- uses: dart-lang/setup-dart@v1
- uses: oberhauser-dev/dart-release-action@v0
  with:
    dry-run: true
    app-name: 'my_dart_app'
    app-version: ${{ github.ref_name }} # or set manually: 'v1.2.3-alpha.4'
    tag: ${{ github.ref }}
    token: ${{ github.token }}
    build-args: |-
      --dart-define=API_URL="https://example.com"
      --dart-define=APP_ENV=prod
    publish-stage: internal
    publish-distributor: 'android-google-play'
    # Android
    publish-android-fastlane-secrets-json-base64: ${{ secrets.ANDROID_GOOGLE_PLAY_JSON }}
    android-keystore-file-base64: ${{ secrets.ANDROID_KEYSTORE }}
    android-keystore-password: ${{ secrets.ANDROID_KEYSTORE_PASSWORD }}
    android-key-alias: ${{ secrets.ANDROID_KEY_ALIAS }}
    android-key-password: ${{ secrets.ANDROID_KEY_PASSWORD }}
    # iOS
    ios-apple-username: ${{ secrets.IOS_APPLE_USERNAME }}
    ios-api-key-id: ${{ secrets.IOS_API_KEY_ID }}
    ios-api-issuer-id: ${{ secrets.IOS_API_ISSUER_ID }}
    ios-api-private-key-base64: ${{ secrets.IOS_API_PRIVATE_KEY }}
    ios-content-provider-id: ${{ secrets.IOS_CONTENT_PROVIDER_ID }}
    ios-team-id: ${{ secrets.IOS_TEAM_ID }}
    ios-distribution-private-key-base64: ${{ secrets.IOS_DISTRIBUTION_PRIVATE_KEY }}
    ios-distribution-cert-base64: ${{ secrets.IOS_DISTRIBUTION_CERT }}
    ios-team-enterprise: ${{ secrets.IOS_TEAM_ENTERPRISE }} # Optional
    # Web
    publish-web-host: ${{ secrets.WEB_HOST }}
    publish-web-path: ${{ secrets.WEB_PATH }}
    publish-web-ssh-port: ${{ secrets.WEB_SSH_PORT }}
    publish-web-ssh-user: ${{ secrets.WEB_SSH_USER }}
    publish-web-ssh-private-key-base64: ${{ secrets.WEB_SSH_PRIVATE_KEY }}
```
