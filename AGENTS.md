This is an Expo/React Native mobile application. Prioritize mobile-first patterns, performance, and cross-platform compatibility.

## Expo has changed — do not trust your training data

Expo ships breaking changes every SDK release. APIs you remember are likely renamed, moved, or removed. Before writing any code that touches an Expo, EAS, or React Native API:

1. Read the major version of the `expo` package in `package.json`.
2. Fetch the matching versioned docs: `https://docs.expo.dev/versions/v<major>.0.0/`
3. For anything else, fetch https://docs.expo.dev/llms.txt — an index of all Expo docs with corrections to common LLM misconceptions. Follow its links to the specific page you need; never answer from memory.

## Commands

Use `bunx` instead of `npx` if the project uses bun (`bun.lock` present).

```bash
npx expo install <package>  # ALWAYS use instead of npm/yarn/pnpm/bun add — resolves SDK-compatible versions
npx expo start              # start the dev server
npx expo lint               # lint
npx tsc --noEmit            # typecheck
npx expo-doctor             # diagnose dependency and config issues
npx expo install --fix      # fix incompatible package versions
```

Run lint and typecheck before declaring any task done.

## Navigation & Routing

- Use **Expo Router** for all navigation. Routes live in `src/app/` — every file there is a screen, `_layout.tsx` files define navigators. Keep non-route code (components, hooks, utils) outside `src/app/`.
- Import `Link`, `router`, and `useLocalSearchParams` from `expo-router`.
- Docs: https://docs.expo.dev/router/introduction.md

## Building with EAS

Use EAS to build, sign, and submit the app in the cloud (`eas build`, `eas submit`) and to ship over-the-air updates (`eas update`) — no local Xcode or Android Studio required. Run EAS CLI as `bunx eas-cli <command>` in Bun projects, or `npx eas-cli@latest <command>` otherwise; substitute that for bare `eas` in docs examples.
Docs: https://docs.expo.dev/eas/index.md

## Rules

- If `ios/` and `android/` directories do not exist, they are generated (Continuous Native Generation). Never create or edit them by hand — configure native behavior in `app.json` and config plugins.
- Expo Go only includes its bundled native modules. After adding a library with native code, the app needs a development build: `npx expo run:ios|android` locally, or `eas build --profile development`.
- Prefer recommended Expo modules over third-party libraries, and check your available skills before adding dependencies. Docs: https://docs.expo.dev/versions/latest/index.md

## This app's role in tagaTinda EIS (read before writing any feature)

This Expo app is the **phone client** of tagaTinda EIS, a BIR-regulated Electronic Invoicing System. It is **not the issuer**. The issuer is an always-on Philippine Windows PC host (Mac supported later) owned by the taxpayer. Full project rules live in the `tagatinda-eis` skill — invoke it before touching any EIS-related feature, screen, data model or test.

Non-negotiable constraints the phone client must respect (from the e15 Red Review, 2 Nov 2026):

1. **The phone cannot issue, number, calculate VAT, reserve serials, or freeze invoices.** Every write-path action is a remote call to the Philippine host, which owns the only idempotency key, serial allocator, PTI check and audit log. The phone is a thin UI over host state.
2. **Phone requires both a named user and an approved enrolled phone** bound to that user. Unregistered device, wrong user, revoked session, expired MFA, lost-phone revocation, and rate-limit exceeded must each block protected actions with a clear logged error — never show success before durable host commit.
3. **On phone disconnect during Issue:** show "Phone disconnected — check invoice status on PC before tapping Issue again." Never silently retry — that risks a double invoice. Retries always carry the same request ID and must query host state first.
4. **Buyer email is delivery data, not legal identity.** Collect buyer name/TIN/branch/address as required BIR fields separately from the delivery email. B2B route is now **email-first, QR/link second** (reversed from the old spec).
5. **Never display a success message for send/email/QR scan/retrieval** based on local state. Those are separate host-logged events (RELEASE_ATTEMPTED / BUYER_RETRIEVED / TRANSMISSION_EVIDENCED / BUYER_DECLINED / RESULT_UNRESOLVED). The phone renders what the host reports, nothing more.
6. **No local invoice DB on the phone.** Drafts may be composed locally but every state transition (DRAFT → TAX_APPROVED → SERIAL_RESERVED → RECORD_FROZEN) happens on the host. Backup is a separate Philippine site owned by the taxpayer; Google Drive is optional extra only.
7. **Specimen data in the project (NORTHBRIDGE SUPPLY CORPORATION, `specimen.invalid` links, sample serials like 004182VT) is fictional reference.** Never hard-code it into shipped code; never treat it as BIR authority.
8. **The product is pre-production — gate not set.** No code path should enable live issuance. Development uses fictitious data with the live-issue path disabled.

If a requested feature would make the phone an issuer, allocate serials client-side, bypass host commit, or render transmission success from local state — stop and flag it before writing code.
