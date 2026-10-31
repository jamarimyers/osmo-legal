# Draft: Ozzy text for the privacy policy (not published)

Removed from the live policy on 2026-10-30 because Ozzy is off in Osmo 1.0.3. Re-add it, updated to the code at that time, **before** `FEATURE_OZZY_CONVERSATION_ENABLED` is turned on for any real athlete, and update `subprocessors/` (add Anthropic) and the App Privacy answers in the same change. This folder starts with an underscore, so the Pages build does not publish it.

Wording last verified against build 24 (`5dd5dd3`, `backend/src/huncho/`). Re-check the Anthropic payload, consent version and memory status before reuse.

## Section 6.A, replace the "AI features" paragraph with:

**AI service provider for Ozzy.** Ozzy, Osmo's AI companion, is optional.

Ozzy works only after you accept a separate in-app acknowledgement for Ozzy conversations (version ozzy-conversation-v2), which you can withdraw in the app. Anthropic, the AI model provider, writes Ozzy's replies. When you send Ozzy a message, Osmo sends Anthropic:

* The text of your message and the recent turns of the current conversation  
* Information Osmo derives for the conversation: the pattern of today's check-in, the titles of practices you did recently, your current streak, and the practices Osmo ranks as suggestions, each with a short reason

In this release, Osmo does not automatically add WHOOP-derived information to the requests it sends Anthropic. Osmo does not add a readiness label, HRV, heart rate, recovery or sleep value, or an indication of whether you have connected WHOOP. Information you include in your messages or current conversation is sent as you wrote it, even if you include WHOOP information. Osmo also does not send your name, email address or account identifier, your journal entries or reflection text, your individual check-in answers, or any audio. If Osmo ever sends WHOOP-derived information to an AI provider, we will update this Privacy Policy before that begins.

Anything you type or dictate to Ozzy is sent as you wrote it, so share only what you are comfortable with. Anthropic handles the text it receives under its own terms and privacy policy, which Stateframe does not control.

Osmo does not save your Ozzy conversation. It is processed to produce a reply and is kept only in the app's memory while the app is open. It is cleared when the app restarts or a different account signs in. If you choose to keep your words, Osmo saves only your own words, after you review and edit them, as a private Journal entry. Osmo's own records of Ozzy use are limited to the analytics events described in Section 3.

Optional Ozzy memory and journal-memory settings, which would let Ozzy remember across conversations, are not available in this release. If a memory feature is added, we will describe it here first, and it will require your separate opt-in.

## One-line items removed elsewhere (put back in the same sections)

- 3.C list: `Messages you send to Ozzy, the optional AI companion (see Section 6)`
- 3.C paragraph: `Dictation. If you use dictation with Ozzy, your speech is converted to text on your device. Osmo does not record, upload or receive audio. Osmo receives only the text you review and send.`
- 3.D, after "Analytics events are kept until you delete your account.": `Events about Ozzy conversations record only fixed categories, such as how a message was entered and a response-time range. They never include the text of your messages or Ozzy's replies.`
- 5 list: `Writing replies in the optional Ozzy conversation, using the information described in Section 6`
- 6.A provider categories: `AI language-model services, for the optional Ozzy companion`
- 7 athlete-private list: `Your messages to Ozzy`
- 9: `Ozzy's replies are written by an AI language model and can be wrong. Ozzy is not a therapist, doctor, counselor or coach, and its replies are not medical advice.`
- 10 Other Osmo records, after the first sentence: `Ozzy conversations are not saved (see Section 6), and dictation audio is not recorded.`
- 11 third-party sentence: list Anthropic again (`WHOOP, Anthropic, Apple, Google`)
- 13 in-app choices: `withdrawing your Ozzy permissions, `
- 15: `Ozzy is an AI companion. It is not a therapist, doctor, counselor, coach or crisis service. If you are in crisis or need urgent help, contact emergency services or a crisis line.`
- 16: `Ozzy's replies are written by Anthropic's AI model, which operates under Anthropic's own terms and privacy policy.`

Kept in the live policy on purpose, because the code still does them: the section 11 deletion step that withdraws Ozzy permissions and asks a memory provider to erase, and the purge-receipt paragraph.
