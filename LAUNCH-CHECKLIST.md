# Breathe Strong: what must be confirmed before the Privacy Policy is final

`privacy.html` is a draft. Every item below shows as "To be confirmed" on the page or as a
`TODO (owner)` comment in the source. Nothing here may be guessed.

## Details to supply
- [ ] Operator: legal name, and whether an individual, sole trader or company
- [ ] Location: state and country, and the legal jurisdiction that applies
- [ ] Privacy contact email (also used on `about.html` under Contact and support)
- [ ] Support contact for the tester
- [ ] Effective date and last-updated date
- [x] Who develops Breathe Strong: Craig, medical scientist, Australia (first name only, by his choice)
- [ ] Legal business name, and the Australian state, for `about.html` and the Privacy Policy

## Facts to verify in the Firebase console (project `breathe-strong-a0c3a`)
- [ ] Database hosting region
- [ ] Database security rules: each user can read and write only their own document
- [ ] Who has administrator access, and whether they can read users' health records
- [ ] Encryption arrangements you are prepared to state
- [ ] Cloud retention period, including inactive accounts
- [ ] Backup retention
- [ ] Scope of "Delete my account data": the tester's code deletes the records document and the
      phone copy and signs the user out; it does not appear to delete the Firebase Authentication
      sign-in record (Google identifier and email). Decide how those are removed.

Checked during drafting: a signed-out request to read a user document was refused by the database
(permission denied). That is one check, not a review of the rules.

## Hosting move to Cloudflare
The draft policy and `about.html` already say the website is hosted through Cloudflare.
- [ ] Connect this repository in Cloudflare (Workers & Pages, create a Pages project, no build
      command, output directory `/`)
- [ ] Confirm Cloudflare Web Analytics is OFF, or update section 11 of the policy to disclose it
- [ ] Replace `https://swornsealed.github.io/breathe-strong-site/` in the `canonical` and `og:url`
      tags of `index.html`, `about.html`, `privacy.html` and `program.html` with the new address
- [ ] Decide whether the tester app also moves. The policy currently says it is served from
      GitHub Pages (sections 2 and 10). If it moves, Google sign-in needs the new domain added in
      Firebase Authentication's authorised domains.
- [ ] Switch off GitHub Pages for this repository, or redirect it, once Cloudflare is live

## Legal review
- [ ] Assess obligations under Australian privacy law for health information; no compliance is claimed
- [ ] Decide whether users outside Australia are actively targeted (EU, UK or other obligations)
- [ ] Confirm there are no other service providers, analytics or tracking tools
- [ ] Remove the `noindex` tag and the draft notice from `privacy.html` when final

## Membership and pricing (not live)
The pricing section on the homepage shows indicative prices and disabled buttons.
See `docs/membership-architecture.md` for the proposed payment set-up.
- [ ] Confirm final prices and currency (indicative: A$19.95 monthly, A$203.49 for 12 months)
- [ ] Decide whether the 12-month plan renews automatically, and state it on the annual card
- [ ] Refund policy and published membership terms (the monthly card refers to "the published terms", which do not exist yet)
- [x] All 52 weeks are released in the app, so the 12-month plan matches the content available
- [ ] Add the payment provider to the Privacy Policy (sections 6 and 10) once chosen
- [ ] Update `about.html` "Tester limitations", which still describes a free tester
- [ ] Build and test checkout, cancellation, notifications and access control before enabling the buttons
