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

## Going live on kingdombeta.com

The site is built from `main` / root by GitHub Pages and previews at `https://prince5670.github.io/reign-of-rivals-site/`. The custom domain is not set yet, because setting it redirects the preview to kingdombeta.com before DNS points here.

1. **Porkbun DNS for `kingdombeta.com`:**
   - **Remove** the apex record pointing at `uixie.porkbun.com` (Porkbun's parking page: an ALIAS that resolves to 207.207.210.x), any URL-forwarding rule on the apex or `www`, and the `www` CNAME to `uixie.porkbun.com`.
   - **Add** A records `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`; AAAA records `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153` (host blank); and CNAME `www` → `prince5670.github.io`.
   - **Keep:** the `beta` A record → `134.122.30.137` (the game server) and both MX records (`fwd1`/`fwd2.porkbun.com`, email forwarding).
2. **GitHub:** repo **Settings → Pages → Custom domain** = `kingdombeta.com` → Save. This commits a `CNAME` file to `main`.
3. Wait for the DNS check to pass and the certificate to be issued (minutes to about an hour), then tick **Enforce HTTPS**.
4. Check `https://kingdombeta.com/privacy`, `/terms`, `/support` and `/delete-account`, and that `https://www.kingdombeta.com` redirects to the apex.
5. Only then set the game server's legal links (`Legal__PrivacyPolicyUrl`, `Legal__TermsOfServiceUrl`, `Legal__SupportUrl`, `Legal__AccountDeletionUrl`) and restart it.

## Editing

- Edit the `.html` files directly. Header and footer are repeated in every page, so a nav change has to be made in all six files.
- All links and asset paths are relative, so the site also works from a sub-path (`https://<user>.github.io/reign-of-rivals-site/`) and on any other static host. The exception is `404.html`, which uses root paths because Pages serves it at any depth.
- When a policy changes, update the "Last updated" date and `sitemap.xml`. For a material Terms change, also raise `Legal__CurrentTermsVersion` on the game server, so players are asked to accept again.

## Sources

The legal text comes from the game repository's drafts (`docs/release/legal/*.md`), checked against the game server's current behaviour on 2026-10-06. Server facts come from `AuthenticationService.DeleteAccountAsync`, `DeletedAccountAnonymizer`, `docs/release/DATA_INVENTORY.md`, `DATA_RETENTION.md`, `PLAY_CONSOLE_CLOSED_TEST.md` and `GAME_DESIGN.md`.

## Assets

- The game art (logo, loading-screen panorama, HUD, League, Gear and chest icons, app icon, feature graphic) is the game's own artwork, resized to WebP.
- The heading font is Fredoka (SIL Open Font License, `assets/fonts/OFL.txt`). It is self-hosted, so the site makes no third-party requests.
