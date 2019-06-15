# Edeno Aidai / Trusted Web Activity

This project uses the
[Trusted Web Activities](https://developers.google.com/web/updates/2017/10/using-twa) technology
to wrap [Edeno Aidai](https://giesmynas.adventistai.lt/) in an Android Application.

## Prerequisites

- Android Studio. For compiling the final Android App Bundle (prefered over APK).
- Digital AssetLinks. For hiding URL bar when using browser custom tabs.

## Guide

Additionally, an amazing guide to follow [Trusted Web Activity - PWA to Play Store Guide](https://fireship.io/lessons/pwa-to-play-store/)

## Digital Asset Links

TWAs require [Digital AssetLinks](https://developers.google.com/digital-asset-links/) to be setup
on both the application and on the website, in order to enable the validation that allows Chrome to
open the page in full-screen.

It requires to have `.well-known/assetlinks.json` file available at PWA's web root that contains the following

```json
[
  {
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {
      "namespace": "android_app",
      "package_name": "org.lukasDev.twa.edenoaidai",
      "sha256_cert_fingerprints": [
        "insert SHA256 hash of Google App signing public key, not the local one"
      ]
    }
  }
]
```

## Debugging Digital Asset Links

As the debug certificate is different from the release one, and the fingerprint for debug should not be listed on the assetlinks.json file, is important to check if your Digital Asset Link is linked and verified.

After you generated your [signed APK](https://developer.android.com/studio/publish/app-signing#sign-apk). it can be installed into a test device, using adb:

`adb install app-release.apk`

If the verification step fails it is possible to check for error messages using the Android Debug Bridge, from your OS’s terminal and with the test device connected.

`adb logcat | grep -e OriginVerifier -e digital_asset_links`

If it is failing you'll see `Statement failure matching fingerprint. Verification failed.` message. Therefore is important to _review_ _AndroidManifest.xml_ and _build.gradle_ files and check if the configurations are matching with the _assetlinks.json_.

Otherwise `Verification succeeded.` message should appear.

--

This repository is formely based upon [SVGOMG / Trusted Web Activity](https://github.com/GoogleChromeLabs/svgomg-twa).
