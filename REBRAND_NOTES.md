# Midnite rebrand notes

Midnite is a derivative of [Helium](https://github.com/imputnet/helium) (GPL-3.0).
This tree is Helium's main repo with user-facing branding changed to Midnite and
**all external dependencies left exactly as upstream** (`deps.ini`, `downloads.ini`,
`flags.gn`, `chromium_version.txt`, `patches/series` are byte-identical).

## What changed
- Product name "Helium" -> "Midnite" in UI strings, comments, docs, i18n
  (source + all translations), issue templates.
- `utils/name_substitution*.py` now substitute Chrome/Chromium -> "Midnite".
- Branding assets from the Midnite pack: app icons, favicons, wordmark logos.
- `product_logo.icon` / `product_logo_color.icon` (Chromium vector icons) and
  the WebUI `chrome-product` path in `helium-logo-icons.patch` regenerated from
  the owl SVG. `product_logo.svg` normalized to 256x256.
- `BRANDING` patch: company/product/copyright -> Midnite; bundle id set to the
  placeholder `com.midnite.browser`; imputnet's Apple Team ID removed.
- Removed upstream-only files: `AGENTS.md`, `.github/.well-known/`.
- CI bot identity changed to `midnite-bot`.

## Intentionally unchanged (renaming would break patches or dependencies)
- `helium://` URL scheme, `helium_*` file/dir names, C++ `helium` namespace,
  `HELIUM_*` macros, pref names, GN targets.
- Copyright headers "The Helium Authors" and the credits entries (attribution).
- `ublock-setup-sources.patch`: context lines must match the uBlock dependency.
- i18n entries owned by dependencies/platform repos (`IDS_HELIUM_ONBOARDING_*`,
  `IDS_KEYCHAIN_STARTUP_ERROR`, `IDS_SETTINGS_DEFAULT_BROWSER_SECONDARY`):
  their English source must match what those repos ship.

## Review before shipping to users
1. **Update feed**: macOS/Windows update checks use `https://updates.helium.computer/`
   (`add-updater-preference.patch`). Point `kUpdateBaseUrl` at your own server, or
   disable updates, so Midnite never follows Helium's release feed.
2. **Crash reports / services** still go to `crash.helium.computer` and
   `services.helium.imput.net`; privacy/terms links point to helium.computer.
   Self-host (helium-services is self-hostable) or disclose.
3. **Onboarding page** (`helium://setup`) comes from the `helium-onboarding`
   dependency and still says "Helium" (~32 strings + logo assets). Fork that repo
   to rebrand it.
4. **Bundle id / signing**: replace `com.midnite.browser`, add your Team ID.
5. **Platform packaging** (helium-macos/-linux/-windows) must be forked for
   installers, .app/.desktop names, icons (.icns/.ico).
6. `CONTRIBUTING.md` and the PR template carry upstream's moderation rules.
7. Wordmark art reads "MidNite"; text uses "Midnite".
8. `legacy.png` / `legacy_crop.png` are included but unreferenced here.

## Verification performed
Same results as the pristine Helium checkout: `devutils/lint.py` 7/7, `i18n_lint`,
`validate_config`, utils tests 6/6, devutils tests 2/2; `i18n.py generate` drift
check exact match; all 346 patches have valid hunks; resource manifests resolve;
`generate_resources.py` runs on the new `raw.png`. Not done: a full Chromium build.
