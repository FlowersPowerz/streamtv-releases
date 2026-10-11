# streamtv-releases

Distribution channel: app packages and update feeds.

One release per platform, under a fixed tag. Its assets are replaced at every publish, so the
download URL an installed app carries never changes.

| Release | Feed | Package |
|---|---|---|
| `desktop-latest` | `version-desktop.json` | `streamtv-macos-aarch64.zip` |
| `android-latest` | `version.json` | `app-android-release.apk` |
| `tv-latest` | `version.json` | `app-tv-release.apk` |

## Recorder

An always-on machine on the home network (NAS, mini PC, Raspberry Pi 4/5) that records instead of the phone.
Container image for x86 and ARM:

```
docker pull ghcr.io/flowerspowerz/recorder:latest
```

**[Install and link it → RECORDER.md](RECORDER.md)** (italiano e English).

No source code here.
