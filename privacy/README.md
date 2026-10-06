# Privacy policies

One page per published app, served by GitHub Pages from this repo at
`https://ivan-i1.github.io/CV/privacy/`.

Play (and the App Store) want a policy URL that names the app it applies to, so
each app gets its own page rather than sharing one.

## Adding a new app

1. Copy the closest existing page — `pinbolita.html` if the app only plays audio,
   `bounce-fidget.html` if it also reads a sensor.
2. Rename it to the app's slug, e.g. `laberinto-de-bolita.html`.
3. Update, in this order: `<title>`, the `<meta name="description">`, the `<h1>`,
   the `.lead` paragraph, the `.app-id` block (both package IDs), the
   **Information stored on your device** list, the **Permissions** section, and
   the `<footer>`.
4. Add a `<li>` to the `policy-list` in `index.html`.
5. Set the `Last updated` date on the new page.

Styling is shared in `style.css`; pages carry no inline CSS, so a change there
applies to all of them.

## Write what the app actually does

Each statement here was checked against the source, not assumed. Before
publishing a page, confirm for that app:

- **No network calls** — grep for `fetch(`, `XMLHttpRequest`, `axios`, and any
  analytics/crash SDK (`firebase`, `Sentry`, `amplitude`, …). If any exist, the
  "collects nothing" claim is false and the page must say what is sent, to whom.
- **What it persists** — list every `AsyncStorage` key and say what it holds.
  Bounce Fidget, for example, does *not* save its settings; Pinbolita does. That
  difference is why the two pages differ.
- **Which permissions reach the built artifact** — read the manifest in the AAB,
  not `app.json`:

  ```
  bundletool dump manifest --bundle app-release.aab
  ```

  `aapt2` cannot read an AAB; the manifest inside is protobuf, not binary XML,
  and `aapt2` fails with empty output that looks like a pass.

A permission in the artifact that the page denies using is a review risk. Either
block it in `app.json` under `android.blockedPermissions`, or explain it on the
page — `bounce-fidget.html` does the latter for the audio-recording permission
that `expo-audio` pulls in.

## Keep it consistent with the Data safety form

Google cross-checks the Play Console Data safety declaration against the linked
policy. If the page says nothing is collected, the form must say the same.
