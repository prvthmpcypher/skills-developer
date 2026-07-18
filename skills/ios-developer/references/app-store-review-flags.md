# Common App Store review rejection triggers

Check generated code and app design against these before considering a feature "done" — these are the most common, avoidable rejection reasons:

- **Sign in with Apple missing** when the app offers other third-party social logins (Google, Facebook) — Apple requires Sign in with Apple be offered as an equivalent option in that case.
- **Inaccurate or missing Privacy Nutrition Label** data relative to what the app actually collects/tracks — this is a submission-form issue, not code, but worth flagging when a feature adds new data collection.
- **Placeholder or broken content** in a submitted build — demo data, "Lorem ipsum," or non-functional buttons will get flagged.
- **Non-standard reimplementation of standard UI patterns** without justification — e.g. a fully custom tab bar that doesn't behave like the system one, without a strong design reason.
- **Use of private/undocumented APIs** — anything not in Apple's public API surface, including symbols accessed via reflection tricks, is an automatic rejection risk.
- **Requesting permissions without a clear, honest usage-description string** — `NSCameraUsageDescription` etc. must accurately describe why the permission is needed, in user-facing language, not a placeholder.
- **In-app purchase bypass** — any digital good/subscription sold outside StoreKit for a use case Apple requires StoreKit for is a common and strictly enforced rejection reason.
