# AltDaemon Modern 1.2.4

## Changes

- Fixes error -22410 ("This action could not be completed. Try again.") that Apple's authentication server began returning on 2026-11-21 for AltStore sign-in and refresh. GSA now refuses SRP authentication when the platform in `X-MMe-Client-Info` differs from the platform of the ADI that produced the anisette data. With on-device AuthKit anisette the ADI is the iPhone or iPad itself, so the previous Mac identity was rejected.
  - SRP `init` and `complete` requests now keep the device's own client info, such as `<iPhone12,3> <iPhone OS;26.0.1;23A355>`, and send the device's akd User-Agent built from its CFNetwork and Darwin versions.
  - The Xcode app token request (`apptokens`) is refused for an iOS identity with -80046, so it uses `<MacBookAir10,1> <Mac OS X;27.3;26C5035r>` with `akd/1.0 CFNetwork/3896.300.31 Darwin/27.3.0`.
  - developerservices2 requests use the same Mac identity as the app token, as before, to avoid error 1100.
  - When the anisette data does not come from the device (anisette v3 fallback), GSA requests still use the Mac identity.
- The previous `Mac15,7` / macOS 15.7.9 identity is no longer used.
- The AltDaemon binary is unchanged from 1.2.3. Only the compatibility module changed.

## Tested configurations

- iPad (7th generation), iPadOS 18.3.2, Dopamine rootless: before the fix, every SRP `init` returned `ec=-22410`. With 1.2.4, sign-in, the app token, and all developer-services requests succeeded, and Refresh All renewed apps to 7 days.
- iPhone 11 Pro, iOS 26.0.1, Dopamine RootHide: every GSA request returned `ec=0` and Refresh All succeeded.

This does not guarantee success on every account, iOS version, or future Apple server change.

## Installation

- Dopamine rootless: install `AltDaemonModern_1.2.4_iphoneos-arm64.deb` over the existing package.
- Dopamine RootHide: install `AltDaemonModern_1.2.4_iphoneos-arm64e.deb`.

Then fully close and reopen AltStore before trying Refresh All. Do not reset anisette state or sign out of Apple ID as a routine update step.

## Credits

Thanks to [ichitaso](https://github.com/ichitaso/AltDaemonModern) for the -22410 fix, and to the iloader and anisette-v3-server contributors for identifying the client info and ADI platform checks ([nab138/iloader#1243](https://github.com/nab138/iloader/pull/1243), [Dadoum/anisette-v3-server#62](https://github.com/Dadoum/anisette-v3-server/pull/62)).

Thanks also to Riley Testut, AltStore, SideStore, and the other contributors. AltDaemon Modern remains open source under AGPL-3.0.
