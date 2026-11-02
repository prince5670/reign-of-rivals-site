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

## Hosting status

- **Live since 2026-10-06** at https://kingdombeta.com. GitHub Pages builds from `main` / root, the custom domain is set (`CNAME`), and **Enforce HTTPS** is on, using a Let's Encrypt certificate for `kingdombeta.com` and `www.kingdombeta.com`. `http://` and `www` redirect to `https://kingdombeta.com/`.
- **DNS (Porkbun):** the apex has GitHub Pages A/AAAA records and `www` is a CNAME to `prince5670.github.io`. Never change the `beta` A record (`134.122.30.137`, the game server) or the MX records (email forwarding).
- **Game server:** `beta.kingdombeta.com` returns these pages from `GET /api/server/legal` (`Legal__PrivacyPolicyUrl`, `Legal__TermsOfServiceUrl`, `Legal__SupportUrl`, `Legal__AccountDeletionUrl`; set 2026-10-06). If a URL ever changes, update the server environment too.
- **Google Play:** Privacy policy URL `https://kingdombeta.com/privacy`; Delete account URL `https://kingdombeta.com/delete-account`.

## Editing

- Edit the `.html` files directly. Header and footer are repeated in every page, so a nav change has to be made in all six files.
- All links and asset paths are relative, so the site also works from a sub-path (`https://<user>.github.io/reign-of-rivals-site/`) and on any other static host. The exception is `404.html`, which uses root paths because Pages serves it at any depth.
- When a policy changes, update the "Last updated" date and `sitemap.xml`. For a material Terms change, also raise `Legal__CurrentTermsVersion` on the game server, so players are asked to accept again.

## Prepared, not published: V10 legal update (branch `claude/v10-legal-update`)

Prepared on 2026-11-02 for the V10 public launch. Publishing is the owner's call at the V10 release. The branch starts from `main` at `e0391e8`, merges `claude/v10-accounts-legal` (`d95b607`, prepared 2026-11-01), and adds the V10 feature changes on top. Publishing this branch publishes both.

**What changes (all pages dated 2 November 2026):**

- **From `claude/v10-accounts-legal`:** web account deletion with a 7-day grace period, the deleted-account retention schedule (30-day purge, reports 1 year, logs 30 days, backups 90 days), optional Google account linking, Unity diagnostics off from 1.7.1, and Terms section 8 without the testing wording. The details are in the game repo, `docs/release/V10_ACCOUNTS_LEGAL.md`.
- **Google linking is worded as "where the game offers it".** The text stays true whether `ExternalIdentity__Google__Enabled` is on or off at launch. The pages affected are Privacy (summary, 2.1, 2.5, 3, 4, 6), Terms section 2, Delete account option 2 and Support.
- **Privacy, new section 2.7 "Notification settings".** It lists the notification kinds (incoming Attack, city lost, pirates landed, Rally/Regroup complete, Alliance Mark, reminders) and explains the per-kind switches, quiet hours and the device time-zone offset. The old 2.7 is now 2.8. The Firebase bullet in section 4 now covers every kind of push and what each one contains. The retention row covers all sent-notification records (about 7 days).
- **Privacy 2.2:** daily login reward claim records, and Season recaps (personal, permanent, only the player sees them; the opponent is stored as a kind, never a name).
- **Privacy 2.3:** Alliance Mark notes (60 characters, filtered like chat, optional chat post). Section 4 says Alliance members see them.
- **Privacy 2.4:** new-player milestone times, used only for totals-only reports. Numbers 1 to 4 and sign-up days with fewer than 5 players are hidden.
- **Privacy, sections 5 and 6, and the Delete account table:** retention for daily reward claims, Season recaps, milestone times, Alliance Marks and notification settings. These match the current server code; see the game repo `docs/release/V10_DATA_SAFETY.md` for the gaps the owner may want fixed first.
- **Privacy 2.5, 8 and 11:** the chosen menu language is stored on the device, notification choices are listed under "Your choices", and the policy is in English only.
- **Terms:** Alliance Mark notes added to the conduct rules (section 4) and to content you create (section 5); a daily login rewards item (7.1); and a line saying the Terms are in English only (section 13).
- `sitemap.xml` lastmod is 2026-11-02 for support, privacy, terms and delete-account.

**Three HTML comments mark text that depends on V10 features shipping.** Before publishing, check each one, delete the comment, and also delete its item if the feature is not in the release:
1. `privacy.html` 2.7: the **Reminders** item (`claude/v10-reminders`, still in progress on 2026-11-02).
2. `terms.html` 7.1: the **Daily login rewards** item (`DailyRewards__Enabled=true` at release).
3. The "available in English only" lines assume the V10 menu translations (`claude/v10-localization`) ship. Without them, the lines are still true and harmless.

**How to publish (owner, at the V10 release):**
1. Deploy the V10 server first: the web deletion page `https://beta.kingdombeta.com/account/delete` and the new notification, Alliance Mark, daily reward, Season recap and funnel tables must exist. Ship the V10 client to players.
2. Do the host retention steps that the policy now promises: journald 30 days, proxy logs 30 days, backups older than 90 days deleted (`V10_ACCOUNTS_LEGAL.md` section 3.2).
3. Resolve the three markers above, then commit.
4. Merge and push. Pushing `main` publishes the site:
   ```bash
   cd C:/Users/princ/reign-of-rivals-site
   git fetch origin
   git checkout main && git pull --ff-only
   git merge --ff-only origin/claude/v10-legal-update   # if main moved since e0391e8, use: git merge origin/claude/v10-legal-update
   git push origin main
   ```
5. Wait 1 to 2 minutes for GitHub Pages, then check that `https://kingdombeta.com/privacy` and `/terms` show "Last updated: 2 November 2026".
6. **Raise the Terms version to 4** (live is 3, set on 2026-10-09), so every player accepts the new Terms again. Use the helper functions from `LAUNCH_DAY_RUNBOOK.md` section 0 (game repo), on the host:
   ```bash
   sudo grep Legal__CurrentTermsVersion /etc/kingdom-game/kingdom-game.env    # expect 3
   envbak; sudo python3 /home/kingdom/set_env.py $ENV Legal__CurrentTermsVersion=4
   sudo systemctl restart kingdom-game && waithealthy
   curl -s http://127.0.0.1:5050/api/server/legal                             # expect "currentTermsVersion":4
   ```
   Do this only **after** step 5, so players never accept a version number whose text is not live yet.
7. Update Play Console Data safety from the game repo `docs/release/V10_DATA_SAFETY.md`.

## Sources

The legal text comes from the game repository's drafts (`docs/release/legal/*.md`), checked against the game server's current behaviour on 2026-10-06. Server facts come from `AuthenticationService.DeleteAccountAsync`, `DeletedAccountAnonymizer`, `docs/release/DATA_INVENTORY.md`, `DATA_RETENTION.md`, `PLAY_CONSOLE_CLOSED_TEST.md` and `GAME_DESIGN.md`.

## Assets

- The game art (logo, loading-screen panorama, HUD, League, Gear and chest icons, app icon, feature graphic) is the game's own artwork, resized to WebP.
- The heading font is Fredoka (SIL Open Font License, `assets/fonts/OFL.txt`). It is self-hosted, so the site makes no third-party requests.
