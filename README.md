# streamtv-releases

Distribution channel: app packages and update feeds.

One release per platform, under a fixed tag. Its assets are replaced at every publish, so the
download URL an installed app carries never changes.

| Release | Feed | Package |
|---|---|---|
| `desktop-latest` | `version-desktop.json` | `streamtv-macos-aarch64.zip` |
| `android-latest` | `version.json` | `app-android-release.apk` |
| `tv-latest` | `version.json` | `app-tv-release.apk` |

The recorder is a container image, `ghcr.io/flowerspowerz/recorder` (x86 and ARM): install and linking in
[RECORDER.md](RECORDER.md).

No source code here.
