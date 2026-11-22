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
- **Price**: suggest a price in EUR based on brand, item and condition, and state your reasoning in one line (e.g. "used Nike Air Max in very good condition usually sells for €35-50 on Vinted NL"). The user decides the final price.
- **Parcel size**: Small (most clothing, light items), Medium (shoes in a box, jackets, jeans in bulk), Large (heavy coats, boots in a box).

Show the full draft to the user and wait for their OK or changes. Do not open the browser until they agree to the draft and the price.

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
