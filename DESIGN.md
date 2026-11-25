# DESIGN.md — GenLayer Project Frontend Standard

Design system for every GenLayer Project frontend (single-file `index.html`, no build step).
Look: calm, flat, sober (PolicyCourt). Structure and UX: ParamCover.
Contract-only submissions have no frontend, so this file does not apply to them.

## 1. Visual theme and atmosphere

- Calm, flat surfaces with thin borders and one soft raised shadow. No gradients, glows or neon.
- Light by default, following the system theme, with a light/dark toggle in the header.
- One signature color per project, chosen for the project's domain, used sparingly.
- Original identity per project. Never copy another product's branding or layout. No fake data anywhere.

## 2. Color palette and roles

Neutrals (shared by every project):

| Role | Light | Dark |
|---|---|---|
| Page background | #F6F6F4 | #121315 |
| Surface (cards) | #FFFFFF | #1A1B1E |
| Surface raised | #FFFFFF | #212226 |
| Border | #E3E3DF | #2B2C31 |
| Text primary | #1B1C1F | #ECECEE |
| Text secondary | #5C5E66 | #A4A6AD |
| Text muted | #8A8C94 | #7A7C84 |

Signature color (one per project, replace per domain):

| Role | Light | Dark |
|---|---|---|
| Signature | #A01B2E (example: seal red) | #FF6B7B |
| Signature tint (10% wash) | signature at 10% alpha | signature at 14% alpha |

Status colors (fixed): success #1F7A4D / #4CC38A, warning #9A6700 / #E5A23C, danger #B42318 / #F97066, info #1F5FAE / #6AA8F0.

The signature color must never be green or teal, and must stay clearly apart from the fixed success green. Pick it from blue, indigo, violet, magenta, red or amber. Never use the signature color to show success, "Open" or "Balanced" states; those always use the status green.

Use the signature color only for: logo, active tab icon, role-card icon tiles, step numbers, highlighted items, primary button. Everything else stays neutral.

## 3. Typography

- Font: Public Sans (or a similar sober sans), system-ui fallback. Set `-webkit-text-size-adjust: 100%`.
- Headline: 26px on phones, up to 36px on desktop, weight 600, line-height 1.2.
- Lede: 15–16px, secondary color, line-height 1.55.
- Body 14–15px, labels 12–13px, never below 11px.
- Weights: 400, 500, 600 only.
- Do not apply `tabular-nums` page-wide (it widens periods in the font). Use it only inside numeric table cells if needed.
- Sentence case everywhere. No ALL CAPS, no Title Case.

## 4. Components

- Buttons: 40px high (minimum touch target 36px), 8px radius. One primary (signature fill) per view; the rest are outlined or ghost. Buttons lock while a transaction runs.
- Cards: 12px radius, 1px border, soft raised shadow (`0 1px 2px rgba(0,0,0,.06), 0 4px 12px rgba(0,0,0,.04)`).
- Item cards: 3px colored top edge for the status color, dot status chips.
- Fixed card anatomy: every item card shows the same fields in the same order (title, status chip, key amount, secondary line, action). Never reorder or hide fields between cards, so users can compare at a glance.
- Role cards: compact row with a 48px tinted icon tile (signature tint, signature icon), title, one-line text, arrow. Border takes the signature color on hover.
- Chips and pills: 999px radius, 12px text, dot + label for status.
- Inputs: 40px high, 1px border, 8px radius, focus ring 2px signature at 40% alpha.
- Dates: short date chips instead of raw date inputs.
- Toasts: bottom on phones, top-right on desktop.
- Confirm dialogs: required for any value-moving or irreversible action.
- Icons: simple outline SVG, 1.5px stroke. Original SVG illustrations for the hero and role cards.

## 5. Layout and information architecture

Mobile-first.

- Phones: bottom tab bar (4 tabs max), action panels first, row lists instead of tables.
- From 1024px: light sidebar with an outlined active item, tables instead of row lists.
- Max content width 1120px, page padding 16px on phones and 24px on desktop.
- Spacing scale: 4, 8, 12, 16, 24, 32, 48.

Home page, in order:

1. Hero: live-status pill, headline, short lede, two CTAs on the left; domain-themed SVG illustration on the right with one entrance animation (a compact 80–96px version beside the headline on phones, full size from 720px). The illustration is original, flat, two tones (signature tint plus signature), and specific to the project's domain. Below it a stats strip with four real stats and colored sublines.
2. "Live contract activity" feed built from real contract data, with time-ago labels and explorer links, ending in a "Balanced" check from the contract's accounting view.
3. Role entry cards with original SVG illustrations.
4. The core action as a short sentence of tappable chip controls. It must be usable on the first screen without scrolling, with live values (amounts, fees, time) shown right next to it.
5. Collapsible four-column how-it-works with signature-colored step numbers.
6. Latest items.

## 6. Elevation and depth

- Flat. One shadow level for cards, one for dialogs and menus. No stacked or colored shadows.
- Dividers are 1px borders, not shadows.
- At most two floating layers on screen at once (menu or toast over a dialog is the limit).

## 7. Transactions and wallet UX

Four-stage tracker on every write: signature → submitted → accepted → finalized.

Separate, human messages for:

- wallet rejection (user cancelled)
- wrong network, with a "Switch network" button
- pre-checked insufficient balance (checked before asking for a signature)
- contract refusal (show the contract's reason)
- refunded bounces (say the value came back)

Rules: buttons lock while running; confirm dialog before value-moving actions; toast on completion.

Costs and risk: show the full amount, any fee or stake, and what comes back, inline next to the action button and again in the confirm dialog, never hidden behind a link. Put a one-line note beside any claim it limits (for example "Studio test network, no real value").

Wallet and contract:

- Connect via browser wallet, test session, or private key.
- Contract address is set in the wallet menu or via `?contract=`. `DEFAULT_CONTRACT_ADDRESS` is filled in after deploy.
- The contract must expose every view the page needs: config, stats, accounting, and lists with timestamps.

## 8. Do's and don'ts

Do:

- Use real contract data for every number, feed row and list.
- Keep one signature color and use it sparingly.
- Give every empty state a short invitation and one action.
- Write UI copy in plain English: verb-first buttons, no "please", no "successfully".

Don't:

- No gradients, glows, neon, glassmorphism or decorative blur.
- No fake or placeholder data in the deployed page.
- No page-wide `tabular-nums`.
- No horizontal scroll at any width from 320px.
- No copy of DoraHacks, BidRoom, ParamCover or any other product's identity.

## 9. Responsive behavior and QA

Breakpoints: 320, 480, 720, 1024, 1440.

QA before delivery (Playwright at 320, 480, 720, 1024, 1440px):

- No horizontal overflow at any width.
- Touch targets at least 36px.
- Light and dark both checked.
- `text-size-adjust` set.
- Every transaction state reachable and readable (rejection, wrong network, low balance, refusal, refund).

Before portal submission: seed the live deployment with real activity so stats and the activity feed are not empty.

## 10. Agent prompt guide

When building a frontend from this file:

1. Pick the signature color for the project's domain and state it first.
2. Build the home page in the order from section 5, mobile layout first.
3. Wire every number to a contract view; if a view is missing, add it to the contract.
4. Implement the transaction tracker and all five error messages from section 7.
5. Run the QA list from section 9 and fix everything before delivering.
6. Deliver code and docs in English only.

Quick prompt:

> Build the frontend for this GenLayer project following DESIGN.md. Signature color: <color + reason>. One `index.html`, mobile-first, real contract data only.
