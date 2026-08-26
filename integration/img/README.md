# Expected image files

Drop these exact filenames into this directory. The page at
`integration/index.html` already references each by this exact path
and name, so nothing else needs to change once they are here.

| Filename | Section it illustrates |
|---|---|
| `01-connect-entry.png` | Connect entry point |
| `02-whoop-authorization.png` | WHOOP authorization consent screen |
| `03-authorization-confirmed.png` | Authorization confirmed |
| `04-connected-state.png` | Connected state in app |
| `05-athlete-data.png` | Athlete view of their own data |
| `06-coach-roster.png` | Coach roster, derived labels only |
| `07-disconnect-flow.png` | Disconnect flow |
| `08-account-deletion.png` | Account deletion |
| `09-legal-access.png` | In-app access to legal documents |

All are referenced as PNGs. If a given screenshot is a JPEG instead,
either convert it or update that one `<img src>` in
`integration/index.html` to match the extension.
