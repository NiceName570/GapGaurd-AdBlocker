# GapGuard — personal ad blocker

First build for **manual testing**, not a claim that every gap between the original blockers is closed. No browser settings or installed extensions were changed during the build.

## Load into Edge

1. Open `edge://extensions`.
2. Enable Developer mode if needed.
3. Click **Load unpacked** and select **C:\Users\crucial\Desktop\ADBLOCKER\dist**. Select `dist`, not the project root.
4. For a clean test, disable the other ad blockers yourself. Keep VPN and Edge tracking settings consistent between comparisons.
5. Open GapGuard's popup, then **Settings & tests**. The status should say **Ready**. If it says **Needs attention**, copy the displayed error.
6. Reload test websites. Already open pages can retain old cosmetics/scripts until reloaded.

Full instructions and results template: [TESTING.md](TESTING.md).

## What is built

- One AdGuard TSWebExtension MV3 engine; no stack of independent blocker engines.
- **Core**: current packaged AdGuard Base, Tracking Protection, Cookie Notices, Popups, and Other Annoyances lists. Base already incorporates EasyList.
- **Combined** (default): Core plus non-duplicate source rules from EasyPrivacy, Fanboy Annoyance, and uBlock filters/privacy/quick fixes/unbreak. Browser conditions and nested upstream includes are resolved during the build.
- Only one compiled profile is enabled at a time. Both packages exist to make Core/Combined comparison easy.
- Network filtering, cosmetic/extended CSS filtering, supported bundled scriptlets and redirect resources.
- Global on/off, per-site exceptions, custom rules, element picker, settings import/export.
- A bounded, memory-only diagnostic log of DNR matches. Its counter is **rule matches**, not an ads-blocked score.
- Optional YouTube helper clicks visible Skip ad buttons. Off by default. It does not prevent unskippable ads or manipulate ordinary playback speed.

OSKA's AdGuard-derived components are covered by the same core rather than copied again. Ad Remover's public upstream filter families inform the supplements. Its proprietary implementation, subscription checks, and bespoke scripts were not copied.

## What "deduplicated" means

Exact filter-text duplicates are removed across the selected inputs. Generated network rules with identical action, condition, and priority are also collapsed; source maps are remapped to retained IDs. Rules with different scopes, exceptions, resource types, or actions are preserved. Different patterns can still cover the same request; complete semantic non-overlap is **not** claimed. Network blocking and cosmetic cleanup also serve different purposes on the same ad.

`reports/build-report.json` records list URLs, hashes, additions, removed duplicates, compiled rule counts, and excluded redirect rules. The extra rules are **coverage candidates** until your real-site tests demonstrate a benefit. More rules alone do not establish better blocking.

## Limitations to record during testing

- Some source rules cannot be represented by MV3 DNR. They are recorded in `reports/*-conversion-errors.json`, not silently advertised as supported. Some cosmetic/script behavior is handled separately by the engine. These reports are compiler exclusions, not a measured count of visible missed ads.
- Engine normalization does not guarantee complete uBlock syntax or scriptlet compatibility. Unsupported redirect targets are excluded and listed in the build report.
- The extension cannot filter internal Edge pages or the add-on store. Test on normal websites.
- Static-rule capacity is shared with other enabled extensions. If startup reports a quota error, disable other ad blockers and reload GapGuard.
- Filters are **bundled snapshots**. This build does not update itself or fetch code at runtime. Rebuild and reload to update.
- Per-site or global changes should be followed by a page reload to undo previously applied cosmetic or script changes.
- The dependency audit contains upstream findings; see `reports/dependency-audit.json` and `reports/DEPENDENCIES.md`. This is a functional prototype, not a completed security review.

## Build again

Node.js 24 is available on this machine. From this folder:

```powershell
npm.cmd ci --no-audit
npm.cmd run build
npm.cmd run check
npm.cmd test
```

Normal builds reuse saved public-list snapshots in `sources/` and the exact locked AdGuard ruleset package. To refresh supplemental lists deliberately:

```powershell
npm.cmd run build -- --refresh
```

Updating the pinned `@adguard/dnr-rulesets` version refreshes the AdGuard inputs. Review a new build report, repeat package checks, and reload the extension. No automatic recurring task is installed.

## Project contents

- `src/`, `ui/`: original integration, controls, optional skip helper.
- `scripts/`: preprocessing, rule conversion, deduplication, checks.
- `sources/`: downloaded public source lists with original headers.
- `reports/`: source hashes, excluded rules, dependency audit.
- `dist/`: the loadable extension; contains bundled code and notices.
- `TESTING.md`: your test links, checks, and a copyable feedback form.

License: GPL-3.0-only for this integration. Upstream credits and licenses are preserved in `THIRD_PARTY_NOTICES.md`, `LICENSE`, and `dist/licenses/`.
