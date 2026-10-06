# Reign of Rivals website

The public website for **Reign of Rivals**: home, support, privacy policy, terms of service, and account deletion. It is a plain static site (HTML and one CSS file, no JavaScript, no build step) served by GitHub Pages at **https://kingdombeta.com**.

| URL | File |
|---|---|
| https://kingdombeta.com/ | `index.html` |
| https://kingdombeta.com/support | `support.html` |
| https://kingdombeta.com/privacy | `privacy.html` |
| https://kingdombeta.com/terms | `terms.html` |
| https://kingdombeta.com/delete-account | `delete-account.html` |

GitHub Pages serves `privacy.html` at `/privacy` with no redirect. Unknown paths get `404.html`.

## Editing

- Edit the `.html` files directly. Header and footer are repeated in every page, so a nav change has to be made in all six files.
- All links and asset paths are relative, so the site also works from a sub-path (`https://<user>.github.io/reign-of-rivals-site/`) and on any other static host. The exception is `404.html`, which uses root paths because Pages serves it at any depth.
- When a policy changes, update the "Last updated" date and `sitemap.xml`. For a material Terms change, also raise `Legal__CurrentTermsVersion` on the game server, so players are asked to accept again.

## Sources

The legal text comes from the game repository's drafts (`docs/release/legal/*.md`), checked against the game server's current behaviour on 2026-10-06. Server facts come from `AuthenticationService.DeleteAccountAsync`, `DeletedAccountAnonymizer`, `docs/release/DATA_INVENTORY.md`, `DATA_RETENTION.md`, `PLAY_CONSOLE_CLOSED_TEST.md` and `GAME_DESIGN.md`.

## Assets

- The game art (logo, loading-screen panorama, HUD, League, Gear and chest icons, app icon, feature graphic) is the game's own artwork, resized to WebP.
- The heading font is Fredoka (SIL Open Font License, `assets/fonts/OFL.txt`). It is self-hosted, so the site makes no third-party requests.
