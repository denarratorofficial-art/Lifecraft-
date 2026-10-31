# LIFECRAFT store: how it works and how to go live

## Platform switch
- The store appears **only** when the platform is `ios` or `android`. Web and laptop builds never show any store UI.
- Detection: Capacitor's `Capacitor.getPlatform()` in the phone apps.
- Testing in a browser: open `index.html?platform=ios` (or `android`). This stays on until you clear
  `localStorage.lc_platform`. Purchases are then **simulated** ("Test mode" banner, no charge).
- In a real store build with no billing bridge connected, purchases are disabled (the mock never runs there).

## Products (create these IDs in App Store Connect and Google Play Console)
| ID | Type | Price | Effect |
|---|---|---|---|
| rescue_s / rescue_m / rescue_l | Consumable | £0.99 / £2.99 / £4.99 | £500 / £2,000 / £10,000 game cash. Only when liquid money < £150, once per 30 game days |
| fresh | Consumable | £1.99 | Clears arrears, +£250, less stress. Needs overdue bills |
| heir | Consumable | £2.99 | Next generation starts with £5,000 |
| supporter | Non-consumable | £4.99 | 10 save slots, badge, 4 accent themes |
| theme_neon | Non-consumable | £0.99 | 2 more accent themes |

## Connecting real billing (RevenueCat or native plugin)
Provide `window.LC_BILLING` before the game loads:

```js
window.LC_BILLING = {
  buy:     async (id) => ({ ok: true, transactionId: '<store transaction id>' }),   // or {ok:false, cancelled:true}
  restore: async () => ['supporter', 'theme_neon']                                // ids of owned non-consumables
};
```
Each `transactionId` is applied once (duplicates ignored). Prices shown in the game are placeholders (`LC_PRODUCTS` in
`window.lcProducts`); update them from the store's localized prices if you prefer.

## Where purchases are stored
- Entitlements (supporter, themes) and unused Heir credits live in `localStorage.lc_iap`, **not** in save files, so
  moving a save or backup between devices cannot unlock them. Cash and Fresh Start effects go into the save by design.
- Saves are editable by players, so for stricter protection add server-side receipt validation (RevenueCat does this)
  before granting.

## Store review checklist
- Visible **Restore purchases** button (included).
- Use only Apple/Google billing for these digital goods; no external payment links in phone builds.
- Privacy policy, age rating (the game has gambling-style content: sportsbook, racing, lottery) and refund wording.
- Gambling money is never sold. Cash purchases are limited to genuinely short-of-money moments, but the money does
  land in the player's bank, so consider age-gating or removing betting in builds aimed at younger audiences.
