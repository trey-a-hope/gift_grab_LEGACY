# Gift Grab (legacy)

The original version of **Gift Grab**, a Christmas arcade game built with Flutter and [Flame](https://flame-engine.org). Players sign in, steer Santa around the screen to grab gifts for 30 seconds, and compete on an online leaderboard and in player groups backed by a [Nakama](https://heroiclabs.com/nakama/) game server. This repo is the legacy app; the game was later split into [gift_grab_game](https://github.com/trey-a-hope/gift_grab_game) and [gift_grab_client](https://github.com/trey-a-hope/gift_grab_client).

## Features

- **The game**: grab gifts for points, avoid ice (frozen for 3 s), pick up flames (10 s of immunity to ice) and cookies (10 s speed boost), with sound effects. Rounds last 30 seconds.
- **Accounts**: email sign-up and log-in with `flutter_login`; sessions are kept with Hive and `flutter_secure_storage` and refreshed when the app resumes.
- **Profile**: pick an avatar, edit your username, delete your account (see `release_notes.json`).
- **Leaderboard**: every finished round writes a leaderboard record; the leaderboard screen lists the top scores.
- **Groups**: create, browse, join and delete groups, and see each group's members.
- **Settings**: app version and profile options.

## How it works

The app uses Riverpod for state and `go_router` for navigation (`lib/data/configuration/app_routes.dart`): a login route, and a main menu with game, leaderboard, groups (create and details) and settings (edit profile) routes.

- `lib/presentation/games/gift_grab_flame_game.dart` and `lib/presentation/components/` hold the Flame game and its sprites.
- `lib/domain/notifiers/` wraps Nakama calls (auth, session, leaderboard, groups, users).
- `lib/data/services/nakama_service.dart` sends email authentication through the [apikama](https://github.com/trey-a-hope/apikama) FastAPI service, which talks to Nakama on the app's behalf.
- The Nakama host, ports and server key live in `lib/data/constants/globals.dart`.

```
lib/
├── main.dart
├── data/            # constants, routes, themes, Nakama/Hive/modal services
├── domain/          # Riverpod providers, notifiers, models
├── presentation/    # Flame game, components, screens, widgets
└── util/            # lifecycle handler, text formatter
assets/
├── audio/           # freeze, flame and item-grab sounds
├── images/          # Santa, gift, ice, flame, cookie, background sprites
└── lotties/         # candy cane and present animations
```

## Tech stack

- Flutter with Dart `>=2.19.6 <3.0.0` (pre-Dart 3)
- `flame` + `flame_audio`
- `nakama` client, `dio`
- `flutter_riverpod`, `go_router`, `hive`, `flutter_secure_storage`, `flutter_login`, `lottie`, `toastification`

## Getting started

You need a Flutter SDK that still supports Dart 2.19 (Flutter 3.7.x), because the SDK constraint stops before Dart 3.

```sh
flutter pub get
flutter run
```

To point at your own backend, change the Nakama settings in `lib/data/constants/globals.dart` and the apikama base URL in `lib/data/services/nakama_service.dart`.

## Status

Legacy and no longer maintained; see `gift_grab_client` for the current app.
