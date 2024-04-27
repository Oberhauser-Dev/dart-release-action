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
    main-path: 'bin/my_dart_app.dart'
    app-name: 'my_dart_app'
    app-version: ${{ github.ref_name }} # or set manually: 'v1.2.3-alpha.4'
    tag: ${{ github.ref }}
    build-args: |-
      --define=DB_USER="user"
      --define=DB_PASSWORD=12345678
    include-paths: |-
      test
      README.md
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
    main-path: 'bin/my_dart_app.dart'
    app-name: 'my_dart_app'
    app-version: ${{ github.ref_name }} # or set manually: 'v1.2.3-alpha.4'
    tag: ${{ github.ref }}
    token: ${{ github.token }}
    build-args: |-
      --define=API_URL="https://example.com"
      --define=APP_ENV=prod
    include-paths: |-
      test
      README.md
    # Deploy
    deploy-web-host: ${{ secrets.WEB_HOST }}
    deploy-web-path: ${{ secrets.WEB_PATH }}
    deploy-web-ssh-port: ${{ secrets.WEB_SSH_PORT }}
    deploy-web-ssh-user: ${{ secrets.WEB_SSH_USER }}
    deploy-web-ssh-private-key-base64: ${{ secrets.WEB_SSH_PRIVATE_KEY }}
    deploy-pre-run: |
      systemctl --user stop my-dart-app.service 
    deploy-post-run: |
      systemctl --user start my-dart-app.service
```
