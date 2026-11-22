# Breathe Strong membership: proposed payment and access architecture

Status: proposal for owner approval. Nothing here is built. The website's trial buttons are
disabled until every item under "Before switching on" is done.

## Principles
- The static website never decides who is a member. It only links to checkout.
- All payment and membership decisions happen on a server the visitor cannot edit.
- No secret keys in any file served to browsers (website or app). Only the payment provider's
  publishable key and price IDs may appear client-side.

## Components
| Part | Role |
|---|---|
| Website (static) | Marketing and pricing. Buttons call the checkout endpoint. |
| App (separate repo) | Asks the server whether the signed-in user is entitled, and shows the program or a paywall. |
| Firebase Authentication | Identifies the member (already used for Google sign-in). |
| Small server (for example Cloudflare Workers or Pages Functions, since the site is moving to Cloudflare) | Holds the secret keys. Creates checkout sessions, receives payment webhooks, records entitlements. |
| Payment provider (proposed: Stripe Checkout and Stripe Billing) | Hosts the payment page, stores card details, runs trials, renewals, receipts and the customer portal. |
| Firestore | Stores one entitlement record per user, writable only by the server. |

## Flow
1. Visitor picks monthly or 12-month on the website. Nothing is preselected.
2. They sign in (Firebase). The website sends the sign-in token and chosen plan to `POST /api/checkout`.
3. The server verifies the token, creates a Stripe Checkout session in subscription mode with a
   7-day trial for the chosen price, tagged with the user's ID, and returns the checkout URL.
4. Stripe's hosted page shows amount, currency, trial length, first billing date, frequency and
   renewal terms, and collects payment details. Card data never touches Breathe Strong systems.
5. Stripe calls `POST /api/stripe-webhook`. The server verifies the signature and writes
   `entitlements/{uid}` with status (trialing, active, past_due, canceled) and period end.
6. The app reads the entitlement. Firestore security rules allow program data to be written only
   while the entitlement is trialing or active, so access is enforced server-side, not just hidden in the UI.
7. "Manage membership" opens Stripe's customer portal through `POST /api/portal` for cancellation,
   card updates and receipts. Cancelling during the trial means no charge.

## Decisions the owner must make
- Final prices and currency (indicative: A$19.95 monthly, A$203.49 for 12 months).
- Whether the 12-month plan renews automatically after a year, and at what price.
- Refund policy, and membership terms covering renewal, cancellation and consumer rights.
- What happens to the app's phone-only mode. Server-side entitlement needs a signed-in account,
  so a no-account mode cannot be checked. Options: require sign-in for members, or keep a limited
  free phone-only mode.
- What existing testers get when the paywall arrives.
- How a 12-month sale is described while only 24 weeks of program exist.
- GST and tax handling, and the business details Stripe requires (ties to the privacy checklist).

## To verify with the payment provider before relying on it
- Availability and requirements for an Australian business or sole trader.
- That free-trial reminder emails can be sent before the first charge for a 7-day trial, and how.
- Customer portal settings for self-service cancellation.
- Card-network rules for free trials (disclosure on the checkout page and in receipts).

## Before switching on
- [ ] Owner decisions above are made and written into published membership terms
- [ ] Checkout, webhook and portal endpoints built and tested in the provider's test mode
- [ ] Entitlement checks enforced in Firestore rules and in the app
- [ ] Cancellation tested end to end, including during the trial
- [ ] Payment notifications (trial ending, receipt, failed payment) confirmed working
- [ ] Privacy Policy updated to name the payment provider and what it receives
- [ ] Website buttons enabled and the "not open yet" notice removed
