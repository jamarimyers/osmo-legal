# App Privacy answers for Osmo 1.0.3 (build 24)

Draft, 2026-10-30. For App Store Connect > App Privacy. Nothing here has been entered in App Store Connect.

**Scope.** These answers describe Osmo 1.0.3 as shipped: iOS build 24, commit `5dd5dd3` in `OsmoPerformance/osmo` (EAS build record, per `docs/release/app-store-1.0.3.md`), with **Ozzy and the memory feature turned off on the server**. Jamari confirmed on 2026-10-30 that the production flags are off, or limited to test accounts. They replace the table in section 4 of `docs/release/app-store-1.0.3.md`, which counted Ozzy messages and Anthropic/Honcho. Each answer was checked against the code at `5dd5dd3` rather than taken from that document. The privacy policy and the new service-provider page in this PR say the same things.

**Not included:** the date of birth typed at the 18+ step. It is checked on the phone and never sent (verified in the release doc and not changed since).

## Top-level answers

| Question | Answer |
|---|---|
| Do you or your third-party partners collect data from this app? | **Yes** |
| Do you or your third-party partners use data for tracking? | **No.** Osmo has no ad SDKs, no data brokers and no third-party analytics. It does not combine app data with other companies' data for advertising. `mobile/package.json` at `5dd5dd3` has no ad, attribution or analytics SDK (only `@sentry/react-native`, `@supabase/supabase-js`, `expo-notifications`, `expo-updates`, `expo-apple-authentication`, `@react-native-google-signin/google-signin`). There is no App Tracking Transparency prompt and none is needed |
| Purposes never to tick | Third-Party Advertising; Developer's Advertising or Marketing |

"Used for tracking" is **No** on every row below. "Linked to identity" is **Yes** on every collected row: either the data sits under the account, or (for Diagnostics) Osmo can match the keyed pseudonym back to an account. Decided by Jamari on 2026-10-30.

## Data types

Vendor key: **SB** Supabase (database, auth, sign-in code email) · **RW** Railway (server host) · **UP** Upstash (job queue, identifiers only) · **SE** Sentry · **EX** Expo (push, OTA updates) · **AP** Apple (Sign in with Apple, APNs) · **GO** Google (Sign in with Google) · **WH** WHOOP (data source, only if connected).

| Apple data type | Collected | Linked | Tracking | Purposes to tick | What it is in 1.0.3 | Vendors | Code evidence |
|---|---|---|---|---|---|---|---|
| Contact Info: Name | **Yes** | Yes | No | App Functionality | First and last name at sign-up; name on the access-request form (before an account exists); name from Apple or Google sign-in if shared | SB, RW, AP, GO | `mobile/features/auth/EmailCode.tsx` (firstName, lastName); `backend/src/routes/accessRequests.ts` (`name`) |
| Contact Info: Email Address | **Yes** | Yes | No | App Functionality | Account email; sign-in code email; access-request email | SB, RW, AP, GO | `accessRequests.ts` (`email`); `mobile/services/auth` (`sendEmailCode`) |
| Contact Info: Phone Number | No | | | | Not asked for | | |
| Contact Info: Physical Address | No | | | | | | |
| Contact Info: Other User Contact Info | No | | | | | | |
| Health & Fitness: Health | **Yes** | Yes | No | App Functionality, Product Personalization, Analytics | Daily check-in (PANAS) answers and emotional-state information; emotional-context answers; pre/post practice state ratings; WHOOP recovery, HRV, resting heart rate, sleep, SpO2, skin temperature and respiratory rate when WHOOP is connected | SB, RW, WH | WHOOP scopes `read:recovery read:cycles read:sleep read:profile offline` (`backend/src/routes/whoop.ts` `WHOOP_SCOPES_ARRAY`); analytics events carry practice responses and impact ratings (`backend/src/data/analyticsEvents.ts`), hence Analytics |
| Health & Fitness: Fitness | **Yes** (Jamari, 2026-10-30) | Yes | No | App Functionality, Product Personalization | WHOOP physiological-cycle data from `read:cycles`: strain, day energy expenditure (kJ), cycle average and maximum heart rate. No workout records (`read:workout` is not requested) | SB, RW, WH | `whoop.ts` scopes; `BIOMETRIC_KEYS` in `backend/src/utils/sentryScrub.ts` lists `cycle_strain`, `cycle_kilojoule…`, `cycle_avg_heart_rate` |
| Financial Info (all) | No | | | | No purchases or payments in the app | | |
| Location: Precise / Coarse | No | | | | No location permission or library. The time-zone name is read from the OS for local-time reminders; Apple does not treat that as Location | | |
| Sensitive Info | **No** (Jamari, 2026-10-30; revisit if Ozzy ships) | | | | Apple's category covers race or ethnicity, sexual orientation, pregnancy, disability, religious or political beliefs, union membership, genetic information and biometric data. Osmo collects none of these as such. Emotional-state and WHOOP data are answered under Health | | |
| Contacts | No | | | | | | |
| User Content: Emails or Text Messages | No | | | | Ozzy is off in 1.0.3. Ozzy messages were never emails or SMS in any case | | |
| User Content: Photos or Videos | No | | | | The app never requests photo-library access | | |
| User Content: Audio Data | No | | | | Dictation is on-device and only reachable through Ozzy, which is off. No audio is uploaded | | |
| User Content: Gameplay Content | No | | | | | | |
| User Content: Customer Support | **Yes** | Yes | No | App Functionality | Weekly feedback and perceived-usefulness answers. Kept from the release doc to be conservative. They could equally go under Other User Content. The "Tell Jamari what to change" note only exists when `FEATURE_PRODUCT_FEEDBACK_ENABLED` is on | SB, RW | `backend/src/data/weeklyFeedbackQuestions.ts` |
| User Content: Other User Content | **Yes** | Yes | No | App Functionality | Journal entries, written reflections, practice notes and ratings; access-request fields (school, team, wearable, how you heard about Osmo). **Changed from the release doc: Ozzy messages removed** | SB, RW | `accessRequests.ts` schema (`school`, `team`, `wearable`, `heard_from`) |
| Browsing History | No | | | | | | |
| Search History | No | | | | | | |
| Identifiers: User ID | **Yes** | Yes | No | App Functionality, Analytics | Osmo account id; Apple and Google sign-in identifiers; WHOOP user id when connected. Keyed pseudonyms of account ids in server error reports | SB, RW, UP, SE, AP, GO, WH | `syncWorker.ts` job data `{ userId, whoopUserId }`; `sentryScrub.ts` `pseudonymize` |
| Identifiers: Device ID | **Yes** | Yes | No | App Functionality | Expo push token (only after the user allows notifications). The Sentry SDK's installation identifier and the update library's installation identifier are device-level random IDs too | SB, EX, SE | `backend/src/services/pushNotifications.ts` (`expo_push_token`); `mobile/services/sentry.ts`; `mobile/app.json` `updates.url` |
| Purchases | No | | | | | | |
| Usage Data: Product Interaction | **Yes** | Yes | No | Analytics, App Functionality | First-party `analytics_events` (feature use, practice completion, streaks), written to Osmo's own database | SB, RW | `mobile/services/analytics.ts`; `/athlete/analytics/event` |
| Usage Data: Advertising Data | No | | | | | | |
| Usage Data: Other Usage Data | No | | | | | | |
| Diagnostics: Crash Data | **Yes** | Yes | No | App Functionality | Native and JS crash reports from the app; error reports from the server | SE | `mobile/services/sentry.ts` (`enableNative: true`); `backend/src/index.ts` `Sentry.init` |
| Diagnostics: Performance Data | **Yes** | Yes | No | App Functionality | Performance traces, 10% sample (`tracesSampleRate: 0.1`) on app and server | SE | same files |
| Diagnostics: Other Diagnostic Data | **Yes** (new vs the release doc) | Yes | No | App Functionality | Release-health session records (`enableAutoSessionTracking: true`), device model, OS version and app version, and breadcrumbs (screens and request paths, query strings removed) attached to reports | SE | `mobile/services/sentry.ts` `scrubBreadcrumb` |
| Surroundings: Environment Scanning | No | | | | | | |
| Body: Hands / Head | No | | | | | | |
| Other Data Types | **Yes** | Yes | No | App Functionality | Optional date of birth and sport saved from Settings > Account; team or institutional affiliation | SB, RW | `mobile/app/(app)/profile/settings/account.tsx` → `PUT /athlete/profile` (`date_of_birth`) |

### What Sentry receives and what is removed (basis for the Diagnostics rows)

Server (`backend/src/index.ts`, `backend/src/utils/sentryScrub.ts`):
- `sendDefaultPii: false`.
- `beforeSend` and `beforeSendTransaction` delete `user.email`, `user.username`, `user.ip_address`, and the `authorization`, `cookie` and `x-whoop-signature` headers.
- `scrubValue` walks the whole event, up to 12 levels deep. It drops every key in `BIOMETRIC_KEYS`: WHOOP values, tokens, `password`, `secret`, emotional states, `reflection`, `content`, `body`, `notes`, `text`, `transcript`, `messages`, `items` and others. It replaces id keys (`userId`, `user_id`, `athleteId`, `coachId`, …) with a truncated HMAC under `SENTRY_IDENTIFIER_HMAC_KEY`. It deletes `request.data`.
- `dropSensitiveRequestBody` drops request bodies for journal, emotional context, practice feedback, lesson reads, Ozzy, team code, signup, provision and team routes.
- Transactions also lose URL query strings, from the request and from every span.
- Exceptions mentioning journal, memory or Ozzy are redacted to a sentinel.

App (`mobile/services/sentry.ts` at `5dd5dd3`, same as current `main`):
- `sendDefaultPii: false`, and no `Sentry.setUser` anywhere in `mobile/`.
- `beforeBreadcrumb` drops console breadcrumbs in release builds and removes query strings from xhr/fetch/http breadcrumb URLs.
- **There is no `beforeSend` in the app.** App error events are not key-scrubbed the way server events are. `mobile/services/api.ts` attaches `{ path, status }` to API errors.
- No Session Replay integration is configured.

## Decisions and open items

Decided by Jamari on 2026-10-30:
- **Fitness: Yes.** WHOOP cycle data (strain, kilojoules, cycle heart rate) counts as fitness data.
- **Sensitive Info: No.** Revisit when Ozzy ships.
- **Diagnostics: linked.**
- **Ozzy is off** in production for real athletes, so the Ozzy text is out of the live policy. The draft is in `_internal/privacy-ozzy-section-draft.md`. Turning Ozzy on means three changes, all made **before** the flag flips: re-add that text to the policy, add Anthropic to `subprocessors/`, and update this label (possible any time in App Store Connect).
- **Sentry "Prevent Storing of IP Addresses" is on.** The policy says so.

Still open:

| # | Item | Status |
|---|---|---|
| O1 | **Sign-in code email sender** | The policy and provider page say Supabase's sign-in service sends the code email. If Supabase > Auth > SMTP uses a custom provider (e.g. Resend), add it to `subprocessors/` and to the Email Address row |
| O2 | **Provider DPAs** | Not yet accepted for Supabase, Sentry, Railway, Upstash or Expo. The policy sentence was softened so it no longer claims contractual obligations. Accept the standard DPAs, then consider restoring stronger wording with counsel |
| O3 | **Access-request retention** | Build 24 deletes nothing automatically. The 90-day sweep is in `OsmoPerformance/osmo` PR 42 (open). The policy says "we plan to". Update the policy when PR 42 ships. Also unverified: whether a request that became an account is removed when that account is deleted |
| O4 | **OTA channel** | Build 24 listens on `testflight`, so OTA updates for testers reach App Store users. Re-check this file before each OTA that adds a data flow |
| O5 | **Sentry region and retention** | Account settings, not in the repo. The policy currently refers to "the period set in Osmo's Sentry account" |

Counsel questions are collected separately for the next counsel meeting.

## Delta from `docs/release/app-store-1.0.3.md` section 4

- Ozzy messages removed from Other User Content. Anthropic and Honcho removed from the vendor picture: Ozzy is off in 1.0.3.
- Fitness changed from No to **Yes** (decided).
- Diagnostics: Other Diagnostic Data added (session records, breadcrumbs, device info).
- Device ID now also names the Sentry and update-library installation ids.
- Access-request fields kept. Their retention is marked pending (O3).
