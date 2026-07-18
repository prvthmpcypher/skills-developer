---
name: android-developer
description: Writes and reviews native Android code in Kotlin/Jetpack Compose (and XML/View-based UI where relevant) — architecture (MVVM/MVI), state management, navigation, and Play Store submission concerns. Use this whenever the user is building a native Android app, asks about Jetpack Compose specifically, mentions Android Studio, Play Store review policies, Android-specific components (Room, WorkManager, DataStore), or needs help with a Gradle/build issue. For cross-platform or generic mobile app planning without an Android-specific need, the general mobile-app-builder skill may fit better.
---

# Android Developer

Modern Android development centers on Jetpack Compose's declarative UI model and Kotlin coroutines for async work — architecture decisions here (state hoisting, ViewModel scoping, unidirectional data flow) shape how maintainable the app stays as it grows, more than any single line of code does.

## Architecture defaults

- **MVVM with unidirectional data flow** is the standard default: `ViewModel` holds UI state (typically as a `StateFlow`), the Composable observes it and sends events back up (not down) via lambda callbacks. Don't let Composables hold business logic or make direct data-layer calls — push that into the `ViewModel` or a repository layer.
- **State hoisting** — a Composable should be stateless where possible, receiving state and callbacks as parameters rather than owning mutable state internally, so it stays reusable and testable. Reserve internal `remember { mutableStateOf(...) }` for UI-only state that has no meaning outside that composable (e.g. whether a dropdown is expanded).
- **Scope `ViewModel`s correctly** — tie them to the navigation graph/screen they belong to via `hiltViewModel()` or equivalent, rather than creating a single app-wide ViewModel that accumulates unrelated state over time.

## Workflow

1. **Check the existing project's patterns first** — Compose vs. XML views, MVVM vs. MVI, Hilt vs. manual DI, before introducing a different pattern into an established codebase. Consistency with what's already there beats an abstractly "better" pattern.
2. **Use Kotlin coroutines and `Flow`/`StateFlow`** for async and reactive state, not the older LiveData pattern, for new code — unless the surrounding codebase is LiveData-based and switching adds interop complexity without a clear win.
3. **Persist data appropriately** — Room for structured relational data, DataStore (not SharedPreferences) for key-value settings in new code, since DataStore is the modern, coroutine-friendly replacement.
4. **Background work** — use `WorkManager` for deferrable, guaranteed background work (e.g. sync, uploads) rather than raw threads or services, since WorkManager correctly handles Doze mode and battery-optimization constraints that raw background work doesn't.
5. **Flag Play Store policy risk proactively** — common triggers: requesting permissions broader than the feature needs (Play Console flags mismatches between declared permissions and actual usage), background location without a clearly justified use case, or missing a Data Safety form entry for new data collection. Say so when relevant.

## What NOT to do

- Don't default to XML View-based UI for new screens in a Compose-based project just because it's more familiar — match the project's actual direction.
- Don't recommend `!!` (non-null assertion) as a default in Kotlin — prefer safe calls (`?.`), `let`, or explicit null handling, reserving `!!` for cases where a null value would represent an actual programming error.
- Don't ignore configuration change handling (rotation, dark mode toggle) — state that should survive these needs to live in a `ViewModel` (which survives config changes) rather than composable-local `remember` state (which doesn't, unless paired with `rememberSaveable`).

## Output format

Provide complete, compilable Kotlin/Compose code with brief comments on non-obvious state-ownership or lifecycle decisions. Flag any Play Store policy risk as a short note after the code.

See `references/play-store-review-flags.md` for common Play Store rejection/flagging triggers.
