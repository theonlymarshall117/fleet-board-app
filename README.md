# Fleet Board (Android)

A thin Android wrapper around your published Fleet Board artifact:
`https://claude.ai/public/artifacts/2a86bb08-c30b-494d-83a3-f8741b04895f`

It's just a full-screen WebView pointed at that link — same data, same
storage, same everything as opening it in a browser. No app-store account
needed for your crew, just an APK they install directly.

## Two kinds of "updates" — know the difference

1. **Content updates** (new features in the Fleet Board itself — board,
   issues, maintenance, inventory, etc.) — you make these in Claude and hit
   **Publish** again on the artifact. Nothing to rebuild here. Everyone
   with the app already installed sees the change next time they open it
   or pull to refresh. This is most of your updates.

2. **App updates** (icon, app name, wrapper behavior, or if you ever change
   which artifact link it points to) — these need a new APK. That's what
   this repo and the GitHub Action are for.

## One-time setup

1. Create a new GitHub repo and push this whole folder to it.
2. That's it — no other setup needed. The Action installs its own Java,
   Android SDK, and Gradle at build time.

## Releasing a new APK

Whenever you change something in this repo (e.g. bump the version or
change the URL in `MainActivity.kt`):

```bash
git add .
git commit -m "Bump to 1.0.1"
git tag v1.0.1
git push origin main --tags
```

Pushing the tag triggers GitHub Actions, which builds the APK and attaches
it to a new **Release** on your repo automatically. Takes a few minutes.

You (and your drivers/mechanics) can then go to your repo's **Releases**
page and download the `.apk` file directly.

You can also trigger a build manually from the **Actions** tab
("Build and Release APK" → "Run workflow") without tagging — that
version gets uploaded as a build artifact instead of a formal release.

## Installing the APK on a phone

Android blocks installs from outside the Play Store by default. First
install only:

1. Download the `.apk` from the GitHub Release on the phone.
2. Tap the downloaded file.
3. Android will prompt to allow installs from that source (Chrome, Files,
   etc.) — allow it once.
4. Tap **Install**.

Future updates: if you release a new APK with the same `applicationId`
(already set to `com.marshall.fleetboard`), installing the new one
upgrades the old one in place — no need to uninstall first.

## Changing the target link

If you ever republish the artifact and get a **new** link (rather than
updating the existing published one), open:

`app/src/main/java/com/marshall/fleetboard/MainActivity.kt`

and update the `appUrl` value, then release a new version as above.

## Building locally instead (optional)

If you'd rather build on your own machine: install Android Studio, open
this folder as a project, and use **Build > Build Bundle(s) / APK(s) >
Build APK(s)**. Not required — the GitHub Action handles this for you.
