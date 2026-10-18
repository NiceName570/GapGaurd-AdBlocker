<p align="center">
  <img src="assets/icons/icon-128.png" width="104" height="104" alt="GapGuard icon">
</p>

<h1 align="center">GapGuard</h1>

<p align="center">
  <strong>Broader filtering. One extension.</strong><br>
  A personal ad and tracker blocker for Microsoft Edge.
</p>

<p align="center">
  Manifest V3 · Version 0.1.1 · GPL-3.0
</p>

---

GapGuard brings established filter lists together in a single extension, removes exact duplicates, and applies them through one filtering engine.

The goal is to close coverage gaps between filter sources while keeping control over what runs. Network blocking, cosmetic filtering, supported scriptlets, and site exceptions work together behind a simple interface.

**Status:** working personal build under active development. Additional coverage is being evaluated through manual testing; combining lists does not guarantee every ad is blocked.

## Features

- **Combined filtering:** AdGuard, EasyPrivacy, Fanboy, and selected uBlock filter families.
- **Duplicate removal:** exact source-rule duplicates and equivalent compiled network rules are collapsed during the build.
- **Two coverage profiles:** switch between Core and Combined to compare behavior.
- **Per-site control:** pause protection for a website or turn it off globally.
- **Element picker:** select an unwanted page element and save a cosmetic rule.
- **Custom rules:** add supported AdGuard-compatible filtering rules.
- **Portable settings:** export and import your settings and exceptions.
- **Local diagnostics:** inspect recent network-rule matches and export a diagnostic summary.
- **Optional YouTube skip helper:** clicks visible Skip ad buttons during ads. Off by default.

## Coverage profiles

| Profile | Included sources |
| --- | --- |
| **Core** | AdGuard Base, Tracking Protection, Cookie Notices, Popups, and Other Annoyances |
| **Combined** | Core plus EasyPrivacy, Fanboy Annoyance, and uBlock filtering, privacy, quick-fix, and compatibility lists |

Combined is enabled by default. Only one compiled profile is active at a time.

The AdGuard Base list already incorporates EasyList. The build preserves rules with different scopes, actions, and exceptions; it does not remove a rule simply because another rule mentions the same domain.

## Build and install

You need **Node.js 24**, npm, and Microsoft Edge.

Download the repository, open a terminal in its folder, and run:

```sh
npm ci
npm run build
npm run check
npm test
```

Then load the extension:

1. Open `edge://extensions` in Edge.
2. Enable **Developer mode**.
3. Select **Load unpacked** and choose the generated **`dist`** folder.
4. Open GapGuard from the extensions menu and check that its status says **Ready**.
5. Reload any websites that were already open.

For clean comparisons, enable one ad blocker at a time.

## Using GapGuard

Open the toolbar popup to toggle protection, pause it on the current site, switch coverage profiles, or launch the element picker.

**Settings & tests** contains site exceptions, custom rules, import/export controls, build information, and diagnostic tools. Reload the webpage after changing protection so previously applied page changes can clear.

The YouTube helper only clicks an available Skip ad button. It does not prevent unskippable ads or alter ordinary video playback speed.

## How the build works

Source lists are saved locally, browser-specific conditions and included files are resolved, and exact duplicate rules are removed. The rules are normalized and compiled for Edge's declarative network filtering API, with supported page filtering handled by the AdGuard engine.

Generated network rules are checked for duplicate actions and conditions. Their source mappings are preserved, and package checks verify that the engine's actual reader can load the generated rule files.

The [build report](reports/build-report.json) records source URLs and hashes, duplicate-removal counts, compiled rule counts, and compatibility exclusions.

### Updating filter snapshots

Normal builds reuse the saved supplemental lists. To fetch fresh supplemental snapshots:

```sh
npm run build -- --refresh
npm run check
npm test
```

The AdGuard snapshots come from the pinned `@adguard/dnr-rulesets` dependency. Updating those requires updating that dependency and rebuilding. After a rebuild, click **Reload** on GapGuard's card at `edge://extensions`.

## Storage and diagnostics

- Settings, custom rules, and site exceptions are saved in browser-local storage.
- The recent network-match log is limited to 100 entries in service-worker memory; query strings and URL fragments are omitted.
- Diagnostic summary exports omit browsing URLs.
- Filters are bundled with the extension. This version has no automatic filter updater and requires no cloud account.

The displayed network-match count includes rule matches such as exceptions and redirects. It is **not an ads-blocked score**.

## Testing and limitations

See the [manual testing guide](TESTING.md) for test sites, comparison steps, and a feedback template.

- Manifest V3 cannot represent every upstream filtering rule. Conversion exclusions are recorded in `reports/`.
- Different rule patterns can still overlap in what they match. Exact deduplication does not prove complete semantic non-overlap.
- Third-party list syntax and scriptlets are not universally compatible with this engine.
- Additional lists can introduce site conflicts. Compare Core and Combined when diagnosing a problem.
- Internal Edge pages and the extension store cannot be filtered.
- Upstream dependency findings are documented in the [dependency notes](reports/DEPENDENCIES.md). This project has not completed a security audit.

When reporting a problem, include the page URL, reproduction steps, and what happens with **Combined**, **Core**, and **GapGuard off**. For screenshots or diagnostic files, review what you share first.

## Credits

GapGuard builds on work from:

- [AdGuard](https://github.com/AdguardTeam/tsurlfilter): filtering engine, DNR conversion, and related libraries.
- [AdGuard Filters](https://github.com/AdguardTeam/AdGuardFilters): core filter lists.
- [EasyList](https://easylist.to/): EasyPrivacy, Fanboy Annoyance, and EasyList rules incorporated by the base filter.
- [uBlock Origin uAssets](https://github.com/uBlockOrigin/uAssets): supplemental filters, privacy rules, quick fixes, and compatibility rules.
- [Mozilla](https://github.com/mozilla/webextension-polyfill): WebExtension API compatibility library.

GapGuard is an independent personal integration, not an official release from these projects. It uses public filter sources and does not include Ad Remover's proprietary implementation or subscription checks.

## License

The GapGuard integration is licensed under **GPL-3.0-only**. Upstream components retain their respective licenses and copyright notices.

See [LICENSE](LICENSE) and [third-party notices](THIRD_PARTY_NOTICES.md).
