GapGuard brings AdGuard core filtering and deduplicated supplemental lists into one Edge extension.

### Downloads

- **GapGuard-Setup-0.1.1.exe**: Windows setup. Places the extension and matching source on your PC and guides loading it in Edge. No administrator access requested. This build is unsigned.
- **GapGuard-0.1.1-extension.zip**: Portable extension. Extract it, open `edge://extensions`, enable Developer mode, choose **Load unpacked**, and select the extracted folder.
- **SHA256SUMS.txt**: Checksums for both downloads.

### Included

- Core and Combined profiles, with only one active at a time.
- Site exceptions, custom rules, element picker, settings import/export, and bounded diagnostics.
- Optional visible YouTube Skip ad button helper, off by default.
- Version 0.1.1 corrects the ruleset metadata mismatch reported during startup.

### Validation and limits

Package checks, source mappings, rule checksums, seven focused tests, and installer payload verification passed. The user confirmed the corrected extension works in Edge. Broader site effectiveness remains manual testing; see TESTING.md. Some upstream syntax cannot be converted to Manifest V3, and dependency findings are recorded in reports/DEPENDENCIES.md.
