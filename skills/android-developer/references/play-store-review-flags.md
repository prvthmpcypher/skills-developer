# Common Play Store policy flags

- **Permission/usage mismatch** — Play Console's automated review flags apps requesting permissions (e.g. `READ_EXTERNAL_STORAGE`, location) not clearly justified by the app's declared functionality. Only request what the feature set actually needs, and for storage access on modern Android, prefer scoped storage APIs over broad storage permissions where possible.
- **Missing or inaccurate Data Safety form** — must match what the app actually collects/shares; a mismatch discovered post-launch can trigger removal, not just a rejection.
- **Background location without clear justification** — one of the most heavily scrutinized permissions; needs a prominent in-app disclosure and a genuinely necessary use case (e.g. not just "better ads").
- **Deceptive or placeholder content** in the submitted build or store listing (screenshots not matching actual app behavior).
- **Target API level requirements** — Play Store enforces a minimum `targetSdkVersion` that rises periodically; an app targeting an old API level can be blocked from new installs even if previously published. Worth checking the current requirement rather than assuming a previously-set target SDK is still compliant.
