# Expected image files

Filenames are what `integration/index.html` references, so a new file
only needs to land here under the exact name below.

| Filename | Section it illustrates | Status |
|---|---|---|
| `01-connect-entry.png` | Connect entry point | **Missing** |
| `02-whoop-authorization.jpg` | WHOOP authorization consent screen | Added |
| `03-authorization-confirmed.jpg` | Authorization confirmed | Added |
| `04-connected-state.jpg` | Connected state in app | Added |
| `05-athlete-data.png` | Athlete view of their own data | Added |
| `06-coach-roster.png` | Coach roster, derived labels only | Added |
| `07-disconnect-flow.jpg` | Disconnect flow | Added |
| `08-account-deletion.jpg` | Account deletion | Added |
| `09-legal-access.jpg` | In-app access to legal documents | Added |

`01-connect-entry.png` still needs a real pre-connection screenshot: a
Profile/Connections view where WHOOP is *not yet* connected, ideally
showing a "Connect WHOOP" affordance. Everything supplied so far
already shows WHOOP connected.

If a new file's extension does not match the table (e.g. you have a
`.jpg` where `.png` is listed), either convert it or update that one
`<img src>` in `integration/index.html` to match.
