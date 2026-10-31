# App Privacy answers for Osmo 1.0.3 (build 24)

Draft, 2026-10-30. For App Store Connect > App Privacy. Nothing here has been entered in App Store Connect.

**Scope.** These answers describe Osmo 1.0.3 as shipped: iOS build 24, commit `5dd5dd3` in `OsmoPerformance/osmo` (EAS build record, per `docs/release/app-store-1.0.3.md`), with **Ozzy and the memory feature turned off on the server**, as Jamari confirmed for this release. They replace the table in section 4 of `docs/release/app-store-1.0.3.md`, which counted Ozzy messages and Anthropic/Honcho. Each answer was checked against the code at `5dd5dd3` rather than taken from that document. The privacy policy and the new service-provider page in this PR say the same things.

**Not included:** the date of birth typed at the 18+ step. It is checked on the phone and never sent (verified in the release doc and not changed since).

## Top-level answers

| Question | Answer |
|---|---|
| Do you or your third-party partners collect data from this app? | **Yes** |
| Do you or your third-party partners use data for tracking? | **No.** Osmo has no ad SDKs, no data brokers and no third-party analytics. It does not combine app data with other companies' data for advertising. `mobile/package.json` at `5dd5dd3` has no ad, attribution or analytics SDK (only `@sentry/react-native`, `@supabase/supabase-js`, `expo-notifications`, `expo-updates`, `expo-apple-authentication`, `@react-native-google-signin/google-signin`). There is no App Tracking Transparency prompt and none is needed |
| Purposes never to tick | Third-Party Advertising; Developer's Advertising or Marketing |

"Used for tracking" is **No** on every row below. "Linked to identity" is **Yes** on every collected row: either the data sits under the account, or (for Diagnostics) Osmo can match the keyed pseudonym back to an account. See counsel item C3.

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
| Health & Fitness: Fitness | **Yes (recommended, see C1)** | Yes | No | App Functionality, Product Personalization | WHOOP physiological-cycle data from `read:cycles`: strain, day energy expenditure (kJ), cycle average and maximum heart rate. No workout records (`read:workout` is not requested) | SB, RW, WH | `whoop.ts` scopes; `BIOMETRIC_KEYS` in `backend/src/utils/sentryScrub.ts` lists `cycle_strain`, `cycle_kilojoule…`, `cycle_avg_heart_rate` |
| Financial Info (all) | No | | | | No purchases or payments in the app | | |
| Location: Precise / Coarse | No | | | | No location permission or library. The time-zone name is read from the OS for local-time reminders; Apple does not treat that as Location | | |
| Sensitive Info | **No (see C2)** | | | | Apple's category covers race or ethnicity, sexual orientation, pregnancy, disability, religious or political beliefs, union membership, genetic information and biometric data. Osmo collects none of these as such. Emotional-state and WHOOP data are answered under Health | | |
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
| Diagnostics: Crash Data | **Yes** | Yes (see C3) | No | App Functionality | Native and JS crash reports from the app; error reports from the server | SE | `mobile/services/sentry.ts` (`enableNative: true`); `backend/src/index.ts` `Sentry.init` |
| Diagnostics: Performance Data | **Yes** | Yes (see C3) | No | App Functionality | Performance traces, 10% sample (`tracesSampleRate: 0.1`) on app and server | SE | same files |
| Diagnostics: Other Diagnostic Data | **Yes** (new vs the release doc) | Yes (see C3) | No | App Functionality | Release-health session records (`enableAutoSessionTracking: true`), device model, OS version and app version, and breadcrumbs (screens and request paths, query strings removed) attached to reports | SE | `mobile/services/sentry.ts` `scrubBreadcrumb` |
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

## Counsel and decision items

| # | Item | Why it is open | Recommended default |
|---|---|---|---|
| C1 | **Is WHOOP cycle data "Fitness"?** | `read:cycles` returns strain, kilojoules and cycle heart rate. The release doc answered Fitness = No because `read:workout` is absent. Apple's Fitness type is "fitness and exercise data", and strain and energy expenditure are arguably that | Tick **Fitness: Yes**. Over-disclosure carries no risk, and under-disclosure is a rejection or enforcement risk |
| C2 | **Sensitive Info** | Apple's list includes "biometric data". Counsel should confirm that this means biometric identifiers (face, fingerprint) and not physiological readings like HRV, and that emotional-state self-reports belong under Health only | **No**, pending counsel |
| C3 | **Diagnostics: linked or not linked** | App events carry no account id, only SDK installation ids. Server events carry an HMAC pseudonym Osmo can re-derive, so Apple would treat them as linked | **Linked: Yes**, conservative (same as the release doc) |
| C4 | **Is Ozzy really off for every real athlete in production?** | The label and the policy both say Ozzy is off. That is true only if production has `FEATURE_OZZY_CONVERSATION_ENABLED` not `true`, or `OZZY_PILOT_COHORT` empty or test accounts only, and `FEATURE_OZZY_MEMORY_ENABLED` off. The repo cannot show production env vars. The Ozzy code ships in build 24 and is turned on server-side, with no new build or review | Jamari to check the Railway env before submitting. Turning Ozzy on later means updating the label (possible any time in App Store Connect) and the policy **before** the flag flips |
| C5 | **OTA channel** | Build 24 listens on the `testflight` channel. Any OTA published for testers reaches App Store users, so an OTA that adds a data flow changes these answers without a review | Process point. Re-check this file before each OTA |
| C6 | **Processor terms** | The policy says providers act "subject to applicable contractual, confidentiality, security, and legal obligations". The repo has no DPA on file for Supabase, Railway, Upstash, Sentry or Expo (`policy-gap-2026-10-02.md` G14) | Collect the standard DPAs, or have counsel soften the sentence |
| C7 | **Sentry settings** | Sentry receives the sender's IP address. Whether it is stored depends on the project's "Prevent Storing of IP Addresses" setting. The data region (US/EU) and the event retention period are account settings and are not in the repo | Turn on IP-storage prevention, then tell me the region and retention so the policy can state them |
| C8 | **Sign-in code emails** | The policy and the provider page say Supabase's sign-in service sends the code email. If a custom SMTP provider is configured in the Supabase dashboard, that provider receives email addresses and must be listed | Check Supabase > Auth > SMTP settings |
| C9 | **Access-request retention** | Build 24 deletes nothing automatically. `ACCESS_REQUEST_RETENTION_DAYS = 90` only flags old rows in the admin list. Automatic 90-day deletion is in `OsmoPerformance/osmo` PR 42, which is **open, not merged**. The policy says "we plan to" and that requests are kept until deleted. Also unverified: whether a request that became an account is removed when that account is deleted | Update the policy sentence when PR 42 ships |
| C10 | **Effective date** | Set to October 30, 2026 in the draft | Change to the actual publish date if publication is later |

## Delta from `docs/release/app-store-1.0.3.md` section 4

- Ozzy messages removed from Other User Content. Anthropic and Honcho removed from the vendor picture: Ozzy is off in 1.0.3.
- Fitness changed from No to **Yes (recommended)**, see C1.
- Diagnostics: Other Diagnostic Data added (session records, breadcrumbs, device info).
- Device ID now also names the Sentry and update-library installation ids.
- Access-request fields kept. Their retention is marked pending (C9).
