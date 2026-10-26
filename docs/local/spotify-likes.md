# Local Spotify likes build

This build applies Philip's Spotify likes contribution from
https://github.com/vorssaint/vorssaint-utils/pull/2750 (original commit
`0a23d55b`) to the local checkout on branch `local/spotify-likes`.
Original authorship is retained. No duplicate upstream PR is needed.

## Scope and integration

Like/unlike the current Spotify song and display its saved state. No library
browser or Spotify queue. The heart shares the current music page's extras row;
the current transport, output picker, and optional volume slider remain.
The integration is optional and uses the PR's browser OAuth PKCE flow and
owner-only local token file, excluded from settings backups. The client ID is
saved in settings and can be included in backups.

## Setup

1. Launch `/Applications/Vorssaint (Developer).app`.
2. Enable **Spotify likes** in Features.
3. Open **Settings → Dynamic Island → Music** and find the Spotify account setup.
4. Create an app at https://developer.spotify.com/dashboard and register exactly
   `http://127.0.0.1:43827/callback` as its redirect URI.
5. Paste its **Client ID** into the app's settings, then connect in the browser.
   No client secret is required. Spotify account/development-mode eligibility is
   governed by Spotify; follow its current dashboard requirements.
6. Play a Spotify song, open the island's music page, and use the heart.
   Verify both save and removal against Spotify's own Liked Songs.

The separate Developer app has its own preferences and macOS permissions.
Its automatic app updater is disabled, so official releases cannot replace it.

## Update procedure

Keep local work committed before updating. Fetch upstream and merge its current
main branch into `local/spotify-likes`, resolve conflicts, then validate and
reinstall:

```sh
git fetch origin
git switch local/spotify-likes
git merge origin/main
./build.sh --test-suite=notch
./build.sh --dev --install
./build/VorssaintDeveloper --selftest
```

Do not reset the local branch to upstream; that discards the customization.
A newer PR revision can be fetched for comparison without replacing local work:
`git fetch origin pull/2750/head:refs/remotes/origin/pr-2750`.
If the PR merges, check whether upstream includes the same changes before
resolving duplicates. Once an official release includes the feature, the
Developer app can be retired in favor of the official app; sign in again there
because the app identities and credential storage are separate.

## Validation limits

Compilation, selftest, and automated tests do not establish that Spotify sign-in
or saving works on this machine. The account owner must finish the browser
connection and verify a live like/unlike. The inherited implementation matches
Spotify's current item to the displayed song by title, which does not distinguish
different recordings with identical titles; review that behavior if it causes
incorrect matches during use.
