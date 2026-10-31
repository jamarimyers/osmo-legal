# **Osmo Service Providers (Subprocessors)**

**Last Updated:** October 31, 2026  
**Applies to:** Osmo app version 1.0.3

This page lists the outside companies that receive information from the Osmo app, and what each one receives. It is part of the [Osmo Privacy Policy](https://legal.osmoperformance.com/privacy/), which explains how Osmo collects, uses and shares information. We will update this page when the list changes.

## **Service providers that process information for Osmo**

| Provider | What it does for Osmo | What it receives |
|---|---|---|
| Supabase | Osmo's database and sign-in service | Everything Osmo stores about your account: profile and sign-in details (a password, if you create one, is stored in hashed form), check-ins, journal entries, practice records, WHOOP data, coach-sharing settings, analytics events and access requests. Supabase's sign-in service also sends the email that carries your one-time sign-in code. |
| Railway | Hosts Osmo's server | The requests the Osmo app sends to Osmo's server, while the server handles them, and the server's operational logs, which use internal account identifiers rather than your name or email address. |
| Upstash | Background job queue for Osmo's server | Internal account identifiers, WHOOP account identifiers and scheduling details for background work, such as fetching new WHOOP data, and short-lived records used while you connect WHOOP. It does not receive WHOOP health values. Finished jobs are removed within 24 hours and failed jobs within 7 days. |
| Sentry | Crash and error reports | Error reports, performance timings and app-session records from the Osmo app and server, filtered as described in Section 6 of the Privacy Policy. Internal account identifiers in server reports are replaced with keyed codes. |
| Expo | Push notifications and app updates | Your device's push token and the text of each notification, if you allow notifications. Osmo's notifications use generic wording and do not include WHOOP values. When the app checks for updates, Expo receives technical details needed to deliver them, such as the app version and platform. |

## **Platforms you choose to use with Osmo**

These companies are not Osmo's service providers. They handle information under their own terms and privacy policies when you choose to use them with Osmo.

| Company | When it is involved | What is shared |
|---|---|---|
| Apple | If you use Sign in with Apple, and when notifications are delivered to your iPhone | For Sign in with Apple, Apple sends Osmo an account identifier and, if you choose to share them, your name and email address. Notifications reach your device through Apple's notification service. |
| Google | If you use Sign in with Google | Google sends Osmo an account identifier, your name and your email address. |
| WHOOP | If you connect your WHOOP account | WHOOP is the source of your WHOOP data, not a provider Osmo sends your Osmo information to. With your authorization, Osmo receives recovery, physiological cycle, sleep and profile information from WHOOP, as described in Section 3 of the Privacy Policy. |

## **Not used in this release**

* **AI model providers.** Ozzy, Osmo's AI companion, is turned off in this release, and Osmo does not send information to an AI model provider. Before Ozzy is offered, we will update this page and the Privacy Policy.
* **Advertising, data brokers and outside analytics.** Osmo does not use advertising networks, data brokers or third-party analytics tools. Osmo's own analytics events are stored in Osmo's database.

## **Questions**

For questions about this list, contact **jamari@osmoperformance.com**.
