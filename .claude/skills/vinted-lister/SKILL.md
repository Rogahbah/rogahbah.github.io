---
name: vinted-lister
description: List one shoe or clothing item on Vinted (vinted.nl) from photos the user uploads. Use when the user shares photos of an item to sell, says "list this on Vinted", "sell this", or invokes /vinted-lister. Writes the listing in English, then fills in the Vinted "Sell an item" form in the user's own logged-in browser and stops before publishing.
---

# Vinted lister

One batch of photos = one product. The user sells in the Netherlands (vinted.nl, prices in EUR) and wants all listing text in English.

## 1. Read the photos

Look at every photo before you write anything. Work out:

- **Item type**: shoes or a clothing type (t-shirt, jeans, jacket, dress...), and men's, women's or kids'.
- **Brand**: from logos, labels, tongue or insole prints. If you can't read it, say "unbranded" and ask the user.
- **Size**: from the label or insole (EU size for shoes; S/M/L or EU/UK/US for clothing). If no photo shows it, ask. Never guess a size.
- **Colour**: the main colour, plus a second one if it's clearly two-tone (Vinted allows two).
- **Material**: only if a care label or obvious material (leather, denim, wool) shows it.
- **Condition**: pick one of Vinted's levels and base it on what you see:
  - New with tags: tags still attached
  - New without tags: unworn, no tags
  - Very good: lightly worn, no visible flaws
  - Good: worn, minor signs of wear
  - Satisfactory: clearly worn, visible flaws
- **Flaws**: stains, pilling, scuffs, sole wear, creasing, holes. Buyers trust listings that name the flaws.

If something important is unclear (size, brand, flaws a photo might hide), ask the user one short list of questions before going on.

## 2. Write the listing

- **Title** (max ~60 characters): Brand + item + key detail + size. Example: `Nike Air Max 90 white/grey sneakers EU 42`.
- **Description** (English, plain, 3-6 short lines): what it is, size and fit, condition with any flaws named honestly, material if known. End with `Smoke-free home. Ships fast.` only if the user has confirmed that is true. No hashtags, no emoji, no all-caps.
- **Price**: work it out with the price check below. Don't put a price in the draft from memory alone.
- **Parcel size**: Small (most clothing, light items), Medium (shoes in a box, jackets, jeans in bulk), Large (heavy coats, boots in a box).

### Price check

Use the browser (see step 3 for which tool) to search Vinted before you suggest a price. Only search and read; don't click anything on a listing.

1. Open `https://www.vinted.nl/catalog` in a new tab and search for the brand and model (e.g. `Nike Air Max 90`). Filter by the same category and, where possible, the same or a close size and the same condition.
2. Pick 5-10 listings that really compare: same brand, same or a very similar model, similar condition. Skip listings that are clearly overpriced or underpriced outliers, and skip "new with tags" listings if the item is worn (and the other way round).
3. Note each one's price (the item price, not including buyer protection or shipping).
4. Suggest two prices:
   - **Recommended**: around the middle (median) of the comparables.
   - **Quick sale**: about 10-20% below that.
   Buyers on Vinted often offer lower, so mention that the recommended price leaves some room to accept an offer.
5. If you find fewer than 3 good comparables, say so, widen the search (similar models from the same brand, any size), and mark the price as a rough estimate.

These are asking prices of items still for sale, not sold prices. Say so when you present them.

Present the price like this and **ask the user to confirm or give their own price**:

```
Price suggestion for <title>
Recommended: €<x>   Quick sale: €<y>
Based on <n> similar listings on Vinted NL (asking prices):
- <title> · <size> · <condition> · €<price>
- ...
Confirm €<x>, €<y>, or tell me your price.
```

Use only the price the user confirms. Never list an item at a price they haven't approved.

Show the full draft (with the confirmed price) to the user and wait for their OK or changes. Don't open the "Sell an item" form until they agree to the draft and the price.

## 3. Fill in Vinted in the user's browser

This step needs the user's own computer, with Chrome logged in to vinted.nl. Use Claude in Chrome (load the `anthropic-skills:chrome-browser` skill first), or computer use if Chrome isn't available. If neither is available (for example in a cloud session with no computer attached), stop and give the user the finished listing to paste in themselves.

1. Open a new tab at `https://www.vinted.nl/items/new`. If Vinted shows a login page, ask the user to log in themselves. Never type their password.
2. **Photos**: upload them in the order the user gave, with the best front shot first. If you can't reach the photo files from the browser (for example because they were only uploaded to the chat), ask the user to drag them into the photo box and tell you when they're done.
3. Fill in title, description, category (click down the category tree until you reach the right one), brand, size, condition, colour(s), material if known, price and parcel size.
4. If the site's language is Dutch, the field labels are: Titel, Beschrijf je artikel, Categorie, Merk, Maat, Staat, Kleur, Materiaal, Prijs, Pakketgrootte.
5. **Stop before publishing.** Take a screenshot or summarise every field, and tell the user to check it and press **Upload / Uploaden** themselves. Never click the publish button yourself, and never change account, payment or shipping settings.

Go at a normal human pace and list one item at a time. Don't bulk-post, don't refresh pages repeatedly and don't touch other listings. That keeps the user's account safe.

## 4. Log it

After the user confirms it's live, add a line to `vinted-listings.md` in the project root (create it if it doesn't exist):

`| date | title | size | condition | price € | status |`

## 5. Check for sales and offers

Use this section when a scheduled check runs, or when the user asks "check Vinted". It needs the browser on the user's computer, as in step 3. Open `https://www.vinted.nl/inbox` and the notifications bell in a new tab. Read them, and act only as described below.

Keep a record in `vinted-state.md` in the project root (create it if it doesn't exist) of every sale and offer you have already reported, by item title and date. That way the same sale or offer is never reported twice.

### When an item has sold

1. Notify the user (push notification): `Sold: <title> for €<price> to <buyer>. Shipping code ready.` (or `...ship by <date>` if Vinted shows a deadline).
2. Open the sale's conversation and get the shipping label:
   - If Vinted offers a choice, **always choose the digital code** (QR code / "no printer needed" / "zonder printer") instead of a label to print.
   - Download the QR code or label and save it on the user's computer as `Vinted shipping/<date> <title>.png` (or `.pdf`). Tell the user where it is and which carrier and drop-off it is for.
   - If the buyer's chosen carrier only gives a printable label, say so and save the PDF.
3. Update `vinted-listings.md`: set the item's status to `sold`.

### When a buyer makes an offer or asks for a lower price

1. Never accept, decline or counter on your own.
2. Notify the user (push notification): `Offer on <title>: €<offer> (listed €<price>). Reply yes or no.` If the buyer asked in a message instead of a formal offer, quote their question in a few words.
3. Wait for the user's answer, then do exactly that in the conversation:
   - **yes**: accept the offer (or, for a message, reply that the price is fine and send an offer at that price if Vinted allows it).
   - **no**: decline the offer, with a short polite message in English: `Thanks for your offer, but I'm keeping the price at €<price>.`
   - If the user gives a different price, send that as a counter-offer.
4. Record the result in `vinted-state.md`.

Don't answer other buyer messages yourself. Report them to the user in one line each.
