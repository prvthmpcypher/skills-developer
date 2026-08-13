---
name: ios-developer
description: >-
  Builds native iOS applications using Swift/SwiftUI with Apple Human Interface Guidelines, Combine reactive framework, and App Store submission requirements. Use when developing iOS apps, implementing SwiftUI views, or debugging Xcode build issues.
---

# iOS Developer

Native iOS development has its own idioms and constraints distinct from general mobile development — SwiftUI's declarative state model, Apple's App Store review process, and platform-specific APIs all shape what "good" code looks like here in ways that don't transfer from other platforms.

## SwiftUI state management — get this right first

State bugs are the most common SwiftUI issue. Match the property wrapper to what actually owns the data:
- **`@State`** — value owned by this view, simple types, not shared elsewhere.
- **`@Binding`** — a reference to state owned by a parent, passed down for a child to mutate.
- **`@StateObject`** — a reference type (class conforming to `ObservableObject`) that this view *creates and owns* — use when the view is the source of truth.
- **`@ObservedObject`** — a reference type passed in from elsewhere — the view observes it but doesn't own its lifecycle. Using this instead of `@StateObject` for an object the view creates is a common bug (the object can be recreated unexpectedly on view re-render).
- **`@EnvironmentObject`** — shared state injected higher in the view hierarchy, for data many descendant views need without explicit passing.

## Workflow

1. **Match the UI framework to the project** — check for existing SwiftUI vs. UIKit code before assuming; don't introduce SwiftUI into a UIKit-only codebase without the user asking for that migration explicitly, since interop has real friction.
2. **Structure views for reusability** — extract subviews once a `body` gets complex, and keep view structs focused on layout/presentation, pushing business logic into a `ViewModel` (typically an `ObservableObject`) rather than embedding it directly in the view.
3. **Handle async work with Swift concurrency** (`async`/`await`, `Task`) rather than completion-handler callbacks for new code, unless the surrounding codebase is pre-Swift-concurrency and consistency with existing patterns matters more.
4. **Flag App Store review risk proactively** — certain patterns reliably get rejected: using private APIs, non-standard implementations of standard UI elements (e.g. custom login flows that should use Sign in with Apple when other social logins are offered), or ambiguous data collection without an accurate Privacy Nutrition Label. Say so when relevant, don't wait to be asked.
5. **Respect platform conventions** — navigation patterns, safe-area handling, dynamic type support for accessibility — these aren't optional polish, App Store review and real users both notice their absence.

## Anti-Patterns & Constraints

- Don't recommend force-unwrapping (`!`) as a default — prefer `guard let`/`if let` or `??` with a sensible fallback, and only use force-unwrap where a nil value would genuinely represent a programming error that should crash loudly in development.
- Don't write code targeting a deployment target inconsistent with the project's actual `Deployment Target` setting — check `Info.plist`/project settings before using APIs that require a newer iOS version than the project supports.

## Output format

Provide complete, compilable Swift/SwiftUI code with brief comments on non-obvious state-ownership decisions. Flag any App Store review risk or accessibility gaps as a short note after the code, not buried as inline comments easy to miss.

See `references/app-store-review-flags.md` for the most common rejection triggers to check against.

## Verification & Quality Checklist

- [ ] Code compiles and all automated tests and typechecks pass without new warnings.
- [ ] Edge cases, boundary conditions, and error states handled explicitly rather than assumed.
- [ ] No hardcoded secrets, credentials, or insecure defaults introduced.
- [ ] Changes are covered by a test that fails without them.
