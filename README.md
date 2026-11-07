# Café QR Menu

A one-file, display-only menu for a café (Arabic + English). Customers scan a QR code on the table and the menu opens on their phone. It has no ordering, no payments and no server code, so it runs free on GitHub Pages or Netlify.

Everything lives in **`index.html`**, plus the logo images **`logo.png`** (gold version for the header) and **`logo-original.png`** (browser tab icon). Upload all three. There are no libraries and no build step. The only outside resource is Google Fonts.

The café details in the file are **sample content** («كامبت هاوس / Gambit House», Karrada, Baghdad). Replace them before going live.

---

## 1. Editing the menu

Open `index.html` in any text editor (Notepad works; VS Code is nicer) and find the block marked
`1. MENU DATA — EDIT THIS PART` near the top of the `<script>`. You never need to touch anything below it.

Every text has two versions: `{ ar: "…", en: "…" }`.
Prices are whole dinars **without commas**: `5000`, not `5,000`. The page adds the commas and the Arabic digits (٥٬٠٠٠ د.ع) by itself.

### CAFE: name, tagline, Instagram
```js
const CAFE = {
  name:      { ar: "كامبت هاوس", en: "Gambit House" },
  tagline:   { ar: "…", en: "…" },
  instagram: "gambithouse_iq",     // without the @
  ...
};
```

### CATEGORIES: the menu sections
Each section has an `id` (items point to it), a `name` and an optional one-line `blurb`. Their order here is their order on the page.

### SIZES: Small / Medium / Large
`add` is added to the item's base price. With `add: 1000` for Medium, a 5,000 latte costs 6,000 in Medium.

### EXTRAS: add-ons (extra shot, oat milk, syrup)
Each extra has an `id`, a `name` and a `price`. An item shows only the extras listed in its own `extras` field.

### ITEMS: the drinks and sweets
```js
{ id: "latte", cat: "hot", price: 5000, sizes: true, extras: ["shot", "oat", "syrup"], badges: [], img: "",
  name: { ar: "لاتيه", en: "Latte" },
  desc: { ar: "…", en: "…" },
  art:  { vessel: "cup", fill: 90, steam: true, layers: [["espresso", 2], ["milk", 5], ["foam", 1.2]] } },
```

| Field | Meaning |
|---|---|
| `id` | Unique name, lowercase, no spaces |
| `cat` | Which section (a `CATEGORIES` id) |
| `price` | Base price in dinar (the Small price if `sizes` is true) |
| `sizes` | `true` shows Small / Medium / Large; `false` means one size |
| `extras` | Which add-ons apply, e.g. `["shot", "oat"]`; `[]` for none |
| `badges` | Any of `"bestseller"`, `"new"`, `"signature"` |
| `img` | Optional photo, e.g. `"images/latte.jpg"`. It replaces the drawing. |
| `hidden: true` | Optional. Hides the item, e.g. when it's sold out. |
| `pick: false` | Optional. The item will never be "Today's pick". |

**Change a price:** edit the `price` number. **Remove an item:** delete its `{ … },` block, or add `hidden: true`. **Add an item:** copy a similar item, give it a new `id`, and change the texts.

### Adding real photos
Create an `images` folder next to `index.html`, put the photo in it (square-ish JPG, around 600×600, under 150 KB), and set `img: "images/latte.jpg"`. If the photo is missing or broken, the drawing shows instead.

### The drawings (`art`)
The drinks are drawn in pure CSS as cross-sections. For a drink you choose a vessel and list its layers **from bottom to top**:

```js
art: { vessel: "glass", fill: 86, ice: true, straw: true, layers: [["condensed", 2], ["milk", 4], ["espresso", 2]] }
```

- `vessel`: `"glass"` (tall iced glass), `"cup"` (latte cup with saucer), `"demitasse"` (small cup with saucer), `"istikan"` (Iraqi tea glass), or `"mug"`
- `fill`: how full the vessel is, 0–100
- `layers`: `[layerName, thickness]` pairs. Thickness is relative: `["milk", 4]` is twice as thick as `["espresso", 2]`. The layer names come from the `LAYERS` list (espresso, crema, milk, foam, condensed, tea, matcha, …). To make a new colour, add a line to `LAYERS`.
- Optional extras: `steam: true`, `ice: true`, `straw: true`, `spoon: true`, `whip: "caramel"` or `"cocoa"`, `garnish: ["mint", "lemon", "date"]`

Sweets use a ready-made shape: `art: { sweet: "cheesecake" }`, `"tart"`, `"tiramisu"`, `"brownie"`, `"kleicha"`, `"croissant"` or `"cookie"`. Variants: `style: "blueberry"` (cheesecake), `style: "cocoa"` (cookie), and `small: true` for a smaller size.

### Buttons and messages
The interface wording (search placeholder, "Today's pick", the footer line…) is in the `TEXT` object, right after the menu data.

---

## 2. Previewing on your computer

Double-click `index.html` and it opens in your browser.

---

## 3. Putting it online with GitHub Pages (free)

1. Create a free account at <https://github.com> (skip this if you already have one).
2. Click **+** → **New repository**. Name it, for example, `cafe-menu`, set it to **Public**, and click **Create repository**.
3. On the new repository page, click **uploading an existing file**, drag in `index.html` (and the `images` folder if you have one), then click **Commit changes**.
4. Open **Settings** → **Pages**. Under *Build and deployment*, set **Source: Deploy from a branch**, **Branch: main**, folder **/ (root)**, then click **Save**.
5. After a minute or two the page shows your address, for example
   `https://your-username.github.io/cafe-menu/`. Open it on your phone to check.

**To update the menu later:** open `index.html` in the repository, click the ✏️ pencil, make the change, and click **Commit changes**. The live site updates in about a minute. The QR code stays the same.

*Alternative, Netlify:* go to <https://app.netlify.com/drop> and drag the folder onto the page. You get an address straight away. Create a free account so you can update the same site later.

---

## 4. Making the QR code

1. Open your live menu address in Chrome on a computer.
2. Click the share / ⋮ menu → **Cast, save and share** → **Create QR code**, then **Download**.
   Any free QR generator also works. Choose one that makes a **static** QR code: "dynamic" QR services often stop working when a free trial ends.
3. Print it at least **3 × 3 cm**, dark on a light background, with a short line such as
   «امسح لعرض القائمة · Scan for the menu». Leave a white margin around the code.
4. **Test it** with two or three different phones before printing a batch for every table.

Because the QR code points to the address, not to the file, you can change prices and items as often as you like without reprinting.

---

## 5. Before going live, check that

- [ ] The CAFE name, tagline and Instagram are real
- [ ] The prices are current
- [ ] The menu opens on a phone, and the language button switches to English and back
