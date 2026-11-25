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
6. Check every item in section 11 (known pitfalls) before delivering.
7. Deliver code and docs in English only.

Quick prompt:

> Build the frontend for this GenLayer project following DESIGN.md. Signature color: <color + reason>. One `index.html`, mobile-first, real contract data only.

## 11. Known pitfalls (from reviewing a shipped project)

Check each one before delivering.

1. First screen: the hero lede is at most two lines. The core action, with its amounts and costs, is visible on the first phone screen. Do not fill the first screen with pill, headline, long text and buttons only.
2. Outcome colors: color a ruling or result by what it means, not by who won. A verdict such as "Buyer won" is neutral or amber, never red. Red is for errors and failures only.
3. Top edge on cards: draw the 3px colored edge with `box-shadow: inset 0 3px 0 <color>` or a clipped pseudo-element so it stays a straight 3px line and does not taper into rounded corners.
4. Section spacing: every section has the same gap above its heading (24px on phones). No heading may touch the card above it.
5. Card anatomy: title, status chip, key amount, secondary line, action button. Join items in the secondary line with a separator (" · "). Never show internal ids such as `deal_1` on a card; keep them for the detail page.
6. Test-network notice: put one line beside the core action and again beside any value-moving button. A footer-only notice is not enough.
7. Tabs: at most four on phones. Move the fifth (for example Account) into the wallet menu.
8. How it works: closed by default on phones, or reduced to a single summary line. Open steps must not push the content below it off the screen.
9. Chip sentences: font 16–17px, never larger than a section heading. Put the chips on their own line under the sentence so they do not break across lines mid-sentence.
10. Stat tiles: labels are one short line (for example "Protected", not "GEN protected now"). Sublines use status colors only, never the signature color.
11. Page height: no `min-height` on the main area that leaves an empty gap above the footer.
12. Contract address: show a short form (first 6 and last 4 characters) with a copy button and an explorer link, not the full string.

Reminder: always judge a layout on a real phone width. A phone browser in "Desktop site" mode shows the wide layout with tiny text, which is not a bug of the page.
