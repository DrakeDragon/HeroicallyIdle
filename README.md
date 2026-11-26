# HEROICALLY IDLE

**An idle RPG where your hero runs, fights and gets into absurd trouble while you're away, and no two heroes ever have the same story.**

Heroically Idle is a single-file mobile web game (one `index.html`, no libraries, no server, no tracking). It is a hybrid of an endless runner and an idle RPG, told as a very funny, very silly LitRPG in the spirit of *Dungeon Crawler Carl* and *We're Off to Be the Wizard*. A tiny pixel hero runs across the top of the screen. Below, a live story feed narrates everything that happens. In between sit three buttons — **Good**, **Neutral** and **Evil** — which you press when the hero faces a dilemma. Close the page and the hero keeps adventuring; when you come back you get a "while you were gone" recap.

Version **1.0 (build v21)** · ~620 KB · needs no network once the page has loaded · designed for phones in portrait (also fine on desktop, landscape and small screens).

![title](docs/screenshots/01-title.png) ![game](docs/screenshots/03-game.png)

---

## Contents

1. [Quick start & deployment](#1-quick-start--deployment)
2. [How to play](#2-how-to-play)
3. [Make It Yours & the Legend Card](#3-make-it-yours--the-legend-card)
4. [The world: 13 realms](#4-the-world-13-realms)
5. [Combat, bosses and loops](#5-combat-bosses-and-loops)
6. [Gear, chests and the shop](#6-gear-chests-and-the-shop)
7. [Companions and pets](#7-companions-and-pets)
8. [Offline play](#8-offline-play)
9. [The story engine](#9-the-story-engine)
10. [Trophies, bestiary, errands, daily gift](#10-trophies-bestiary-errands-daily-gift)
11. [Automation (the gnomes)](#11-automation-the-gnomes)
12. [Saving, backups and privacy](#12-saving-backups-and-privacy)
13. [Balance and pacing](#13-balance-and-pacing)
14. [Technical notes & project layout](#14-technical-notes--project-layout)
15. [Testing](#15-testing)
16. [Known gaps (honest list)](#16-known-gaps-honest-list)
17. [Version history](#17-version-history)

---

## 1. Quick start & deployment

**Play locally:** open `index.html` in any modern browser. That's it.

**Host on GitHub Pages (free):**
1. Create a new public repository (for example `heroically-idle`).
2. Upload `index.html` to the **root** of the repository (keep the name `index.html`).
3. Go to **Settings → Pages**, choose **Deploy from a branch**, select `main` and `/ (root)`, and save.
4. After a minute the game is live at `https://<your-username>.github.io/heroically-idle/`.
5. On a phone, open the link and use *Add to Home Screen* to get an app-style icon. The page sets the right web-app tags for iOS and Android.

You do not need to upload the `source/`, `tests/` or `docs/` folders to host the game; they are for development and documentation.

Saves are stored in the browser (`localStorage`), per device and per web address. Moving the game to a new address starts a fresh hero unless you use the built-in **Save backup** (section 12).

---

## 2. How to play

**The screen has four parts, top to bottom:**
1. **The runway (top ~43%).** Your pixel hero runs through the current realm. Coins float past, enemies appear. **Tap the top half of the screen to jump.** You collect coins by running through them and dodge or jump enemies. Coins start tiny (half a penny!) and grow through upgrades and realms.
2. **The status panel.** Hero name (tap to rename), level, XP bar with time to next level, title, party line (companions and pet), and your coins. Small buttons: **profile**, **menu**, **sound (♪)**, **text size (Aa)**, **game speed**, and **restart**.
3. **The dilemma strip.** Every minute or so the hero hits a moral dilemma. The three buttons — **Good** (gold), **Neutral** (cyan), **Evil** (magenta) — light up. Whichever you choose changes the outcome, gives different loot and consequences, and pushes the hero's **alignment**, which then biases the kinds of events that happen (good heroes meet more do-gooders, evil heroes get black-market discounts at chests, and so on). If you don't press anything the game picks according to your alignment, so you never have to watch it.
4. **The feed (bottom).** The running story: fights, dilemmas, side quests, bosses, loot, system messages from the narrating "System", trophies, offline recaps. Lines are tagged with colored bars by tone.

**The loop:** run → collect coins → spend coins in the **Shop** (pink button on the runway) → get stronger → push through more levels → meet bosses → advance to the next realm.

**Levels and stages:** each realm has **300 levels**, each level has **10 stages**, and every stage is about 520 distance units of running. The last stage of a level ends with a boss (champion, guardian or realm boss — see section 5).

**Speed:** the speed button cycles real-time, ×4, ×20, ×100, ×400 for impatient moments. Offline time is always counted at real-time.

**Controls summary**

| Action | How |
|---|---|
| Jump | Tap the top half of the screen (a Double Jump upgrade allows a second hop) |
| Choose Good / Neutral / Evil | Tap the matching button when lit |
| Open shop | Pink SHOP button on the runway |
| Profile, trophies, errands, codex, automation | 👤 and ☰ buttons |
| Sound on/off | ♪ button (remembered between visits) |
| Rename hero | Tap the hero's name |
| Make It Yours / Legend Card | Profile → bottom buttons |

---

## 3. Make It Yours & the Legend Card

This is the feature that makes every hero personal.

### Make It Yours
On your first visit (and any time from **Profile → MAKE IT YOURS**) you can optionally set:
- **Hero name.**
- **Pronouns:** he / him, she / her, or they / them. The story text is written around "he", so the game rewrites pronouns for the hero on the fly (conjugating verbs for "they": "they ask", "they're"). It is deliberately conservative: when a sentence also features another character (a guard, a nemesis, your pet…) it leaves the pronoun alone rather than risk pointing it at the wrong person. A few lines will therefore still say "he" for a she/they hero; see Known Gaps.
- **Your catchphrase:** what your hero shouts. It becomes the hero's signature line in dozens of events and on the Legend Card.
- **Your pet's name.** The pet type (goose, goat with a pension, ghost who is mostly polite…) is still random.
- **Your nemesis's name.**
- **A friend's name.** They will turn up as an NPC, a companion, a side-quest character. Putting a real friend or relative here is the funniest thing in the game.

Text you type is cleaned (no markup or braces, 32 characters max) and written into the story. Everything is optional; Skip is always available.

### Story DNA
Independently of what you type, every new hero is rolled a unique **Story DNA**: a title the System calls you, an origin story, a known flaw, something you are never without, a catchphrase, a fear, a patron organisation, a companion-for-life pet, a grudge the nemesis holds, and a secret. These threads recur through the story, and some twists change them for real (see 9.3). In a test of 2,000 freshly rolled heroes, all 2,000 were different.

### The Legend Card
**Profile → LEGEND CARD** creates a shareable 600×960 picture of your hero: a snapshot of the current scene (with your hero, gear and pet), name, title, level and realm, flaw, catchphrase, companion, nemesis, fear, patron, win/quest/side-quest/statue counts, and a **Legend number** (for example `13EF-U3D`) that is derived from the hero's DNA. On phones that support it the **Share / Save** button opens the share sheet; otherwise it downloads a PNG. You can also long-press the preview to save it.

![make it yours](docs/screenshots/02-make-it-yours.png) ![legend card](docs/screenshots/06-legend-card.png)

---

## 4. The world: 13 realms

Each realm has its own sky and palette, props, weather, three foes (each with its own hit/dodge/kill lines), NPCs, signs, rumors, loot, hazards, food, 300 uniquely named levels with level NPCs and quirks, intro lines, and a boss. The realms are traveled in order and, after the last, the game loops into a **New Game+** with stronger enemies and richer coins (section 5).

| # | Realm | Foes | Boss |
|---|---|---|---|
| 1 | Meadowshire | Angry Hedge, Passive-Aggressive Scarecrow, Tax Goose | Sir Reginald Hedgewick — *Lord of Unsolicited Trimming* |
| 2 | Pinewhisper Woods | Judgmental Stump, Lumberjack's Ghost, Owl With Opinions | The Great Stumpton — *Elder Judge of the Woods* |
| 3 | Cactus Flats | Tumbleweed With Anger Issues, Sunburnt Lizard, Mirage Salesman | El Tumbleweedo Supremo — *Rolling Menace of the Flats* |
| 4 | Frostbite Flats | Snowman Union Rep, Icicle Gremlin, Penguin Mafia | Chairman Frostbeard — *President of the Snowmen Union Local 3* |
| 5 | Mushroom Meh-dows | Sporeling Bouncer, Gloomy Toadstool, Hallucination (Probably) | Mother Spore — *Who Is Definitely Real* |
| 6 | Candy Cavity | Sentient Gumdrop, Dentist Golem, Licorice Viper | The Molar Monarch — *Sovereign of the Sticky Throne* |
| 7 | Crystal Caverns | Geode Goblin, Echo Bat, Stalagmite Preacher | Lord Geodius the Faceted — *Keeper of the Echoing Deep* |
| 8 | Bone Orchard | Skeleton Intern, Fussy Ghoul, Grave Robber (Retired) | The Skeleton Regional Manager — *Overseer of the Bone Orchard* |
| 9 | Brimstone Basement | Imp Middle Manager, Lava Slug, Hellhound Puppy | Gary — *Vice President of Hell (Acting)* |
| 10 | Sunken Soggy Reef | Crab With a Grudge, Jellyfish Lawyer, Anchor Thug | Admiral Barnaby Claws — *Terror of the Tide Pools* |
| 11 | Cloud Cuckoo Land | Cherub With a Bill, Harp Hooligan, Cumulus Karen | Cumulus Maximus — *Senior Management Cloud* |
| 12 | The Void-ish | Existential Dread (Small), Paradox Cat, Void Janitor | The Void Janitor's Boss — *Nobody Has Ever Met Him* |
| 13 | The Unfinished Draft | Rough Sketch, Typo Wraith, Unresolved Plot Hole | THE EDITOR — *Who Has Seen Your Draft* |

Finishing a realm boss opens the next realm and plays that realm's Grand Plot finale (section 9.2).

---

## 5. Combat, bosses and loops

Combat is automatic: the hero fights whatever it runs into. Damage scales with **hero power**:

`hero power = gear power + 4 × level + companion bonus`

Enemy health is clamped relative to your power so fights never take absurdly long or end instantly (typical fights last 10–75 seconds, never more than about a minute and a quarter). Boss fights were tested in realms 1, 4, 8 and 13.

**Boss tiers**
- **Champions** end every ordinary level (the 10th stage is a boss stage): a rare-or-better version of one of the realm's foes.
- **Guardians** end every 10th level: bigger, with names like "Bumbling Judgmental Stump", "Merciless Judgmental Stump, Again"…
- **Realm bosses** end level 300 of each realm. Each has a full intro scene, a title, drops guaranteed loot, and triggers a Grand Plot chapter. The 13 are listed in the realm table above.

Hits against you cost coins instead of health (boss hits cost triple), so being hit stings but never ends your run. **Second Wind** (upgrade) reduces that loss.

**Momentum:** staying in the flow of fights builds a momentum bonus (max +30%, up to +57% with the Momentum upgrade) to your pace.

**Loops (New Game+):** after The Unfinished Draft the game starts over at Meadowshire with a **loop** counter. Coins get +3 realm-multiplier points per loop, gear power grows with each realm and loop, and special Grand Plot openers play for loops 2 and up.

---

## 6. Gear, chests and the shop

**Equipment:** the hero wears gear in **10 slots** (helm, amulet, ring, tunic, shield, weapon, gloves, leggings, boots, charm — shown as icons on the runway). Each item has a **rarity** — Common, Uncommon, Rare, Epic, Legendary — a unique funny name ("Skeever's Gleaming Flail", "Amulet of Zero Protection (It's Mostly for Looks)"), and a power value. Better items auto-equip; weaker ones are sold. Rare and better items add visible flourishes to the hero sprite (plumes, pauldrons, gold trim, sparkles). Legendary foes drop with their own rare sprites.

**Shop** (pink SHOP button). Three tabs:

**UPGRADES**

| Upgrade | Max level | What it does |
|---|---|---|
| Coin Value | 40 | Each coin is worth more. |
| Coin Magnet | 6 | Coins drift toward you. |
| Springy Boots | 5 | Jump higher and farther. |
| Double Jump | 1 | Jump again in mid-air. |
| Auto-Hop | 5 | Hops over the truly tough enemies. |
| Sword Speed | 6 | Swing faster in fights. |
| Coin Density | 6 | More coins show up on the road. |
| Running Shoes | 6 | Raises the cap on Momentum: clear road makes you faster. |
| Lucky Rabbit's Foot | 5 | Chests roll better odds. |
| Boss Slayer Manual | 25 | Hit bosses harder. |
| Coin Banker Gnome | 25 | Earn more coins while you are away. |
| Squad Goals | 12 | Companions add more to your power. |
| Fan Club Perks | 25 | Your fans make each coin worth more. |
| Study Habits | 20 | Level up faster. |
| Second Wind | 10 | Enemy hits cost you fewer coins. |
| Golden Ledger | 500 (endless, appears in realm 3) | Accountants for your accountants. Coins worth more, forever. |
| Training Fund | 500 (endless, appears in realm 3) | A grant for hitting things more educationally. |
| Guild Banner | 500 (endless, appears in realm 3) | A big flag. Brings in coins while you are away. |

**CHESTS** — four tiers: Suspiciously Damp Chest, Iron-ish Chest, Gilded Chest of Bad Ideas, Mythic Lockbox (Probably Cursed). Each opens to a random piece of gear at better odds than the road. The **Luck** upgrade shifts odds upward. Duplicates and downgrades sell back for 15%. Evil heroes get a discount. Chest prices scale with both level and your coin value, so they stay meaningful at any stage.

**LEGACY** (appears in realm 3) — the late-game coin sinks:
- **Commission a Bard** — pays a bard to start a brand-new side quest on the spot.
- **Statue of your hero** — one per realm and loop. A big fan surge and **+4% coin value forever** per statue.
- **Sponsor a Festival** — coins worth double for 10 minutes plus a fan surge.

The three endless upgrades (Golden Ledger, Training Fund, Guild Banner) cost a multiple of your current income, so each level costs minutes to hours of earning regardless of how rich you are.

![shop](docs/screenshots/04-shop-upgrades.png) ![legacy](docs/screenshots/05-shop-legacy.png)

**Fans** are a hidden currency of fame. Side quests, statues, festivals and twists raise them, and Fan Club Perks turns them into coin value.

---

## 7. Companions and pets

- **Companions** join for a while and then move on. Up to **3** can travel with you. They are named, absurd ("a toddler named Cuthbert", "Mort, an adventurer with a bad knee", "a dragon hatchling (embarrassed)", "an orc accountant", "Gerald, a raccoon in a cape") and each adds to hero power (**Squad Goals** boosts this).
- A companion stays for a **progress-based** stay (about 100 levels on average in testing, measured in levels completed, so offline time never burns a companion's life) and then departs with a farewell line; departures are listed in your profile ("Moved on or lost…").
- Your **pet** is part of your Story DNA ("Biscuit the goat with a pension"). It walks beside your hero on the runway, has its own small sprite, and has plot-lines, including one twist where it turns out to be a dragon.
- A named companion can also come from side quests (for example the Chosen Intern) or twists (a long-lost aunt).
- If you entered a friend's name in Make It Yours, they are mixed into the pool of NPC names, so they turn up as shopkeepers, quest-givers and side-quest characters.

---

## 8. Offline play

When you return after at least 20 seconds away, the game simulates what happened (up to **48 hours** of absence at a time):
- Your hero runs on at a fraction of active pace, earning coins (boosted by **Coin Banker Gnome**, **Guild Banner** and **Run Speed/Momentum**), XP and loot.
- Levels and realms can advance while you're away.
- The shop and chest gnomes keep buying on your behalf if switched on.
- Story events are generated in the background and the best moments (important lines first, then a representative sample) are replayed as a recap, including up to a handful of **side-quest beats** so the plot moves while you're gone.
- Grand Plot chapters you skipped over are delivered later as "FLASHBACK" chapters.

Offline pacing is deliberately slower than active play, so the game is comfortable at an hour a day.

---

## 9. The story engine

The aim: no two players read the same story. The engine combines hand-written scenes with templates, word banks and long-running plots. Roughly **684,304,793,482,998,700 "possible tales"** is the (very rough, combinatorial) number the profile screen quotes.

### 9.1 Text engine
Every line is a template filled from word banks: `{hero}`, `{comp}`, realm slots (`{rname}`, `{rfoe}`, `{rnpc}`, `{rweather}`, `{rrumor}`…), level slots (`{lname}`, `{lnpc}`, `{lquirk}`) and your Story DNA (`{dflaw}`, `{dcatch}`, `{dpet}`…). Lines come in three **tones** (Good, Neutral, Evil) and respond to your alignment.

**No-repeat guarantee.** The game remembers what it said recently: random choices avoid recently used options, and a line identical to one in the last ~260 lines is suppressed. In a simulation, repeats within 150 lines dropped to zero. Grammar clean-ups run on every line (a/an, doubled words, double periods, sentence capitalisation, doubled place names).

### 9.2 The Grand Plot
A 13-chapter-set main story (4 beats per realm, 52 beats, plus flashbacks and New Game+ openers) that unfolds as you play. The mystery of **"F"** — a note-writing mystery sender — leads through a hidden gallery of previous Chosen Ones, a skimming accountant, the Vice President of Hell (Acting) who "has been doing the metrics", an Archive of every hero's story that ends in "and then the story stopped", an audience made of former side characters, and finally **THE EDITOR**, who turns out to have been the original hero. (No spoilers beyond that.)

### 9.3 Side quests

Side quests are multi-beat micro-stories that interleave with the main feed and finish with a resolution, a coin reward, a fan surge and a **memory** that later events can call back. The game picks from several sources:

- **39 realm-specific quests** (3 per realm, 4 beats each):
- **Meadowshire:** The Great Hedge Dispute; The Scarecrow Union; The Wedding That Never Was
- **Pinewhisper Woods:** The Trial of the Great Stumpton; The Owl With The Questions; The Lumberjack Who Cried Timber
- **Cactus Flats:** The Tumbleweed Escape; The Cactus Who Wanted A Hug; The Saloon That Moves
- **Frostbite Flats:** The Snowmen Strike; The Lost Mitten; The Ice Fishing Tournament
- **Mushroom Meh-dows:** Mother Spore's Lullaby; The Mushroom Census; The Glowing Rumour
- **Candy Cavity:** The Molar Monarch's Checkup; The Gummy Mutiny; The Great Sugar Rush
- **Crystal Caverns:** The Echo Problem; The Geode Heist; The Glow-Worm Union
- **Bone Orchard:** The Skeleton Regional Manager's Review; The Orchard Harvest; The Ghost Who Forgot
- **Brimstone Basement:** Gary's Acting Vice Presidency; The Paperwork Pit; The Barbecue From Hell
- **Sunken Soggy Reef:** Admiral Claws' Treasure Map; The Coral Council; The Message In A Bottle
- **Cloud Cuckoo Land:** Cumulus Maximus' Performance Review; The Rainbow Toll; The Thunder Choir
- **The Void-ish:** The Void Janitor's Request; The Lost Dimension; The Echo Of A Hero
- **The Unfinished Draft:** The Editor's Red Pen; The Plot Hole; The Last Draft
- **29 general quests** such as The Pie Incident, The Subscription, The Lawsuit, The Accidental Wedding, The Haunted Thing, and The Annoyed Wizard, The Haunted Inn, The Dragon's Therapist…
- **12 calendar quests**, each offered about once a year in its month (it uses the date on your device): January *The Resolution*, February *The Valentine's Mix-Up*, March *The Spring Cleaning*, April *The April Fool*, May *The May Pole*, June *The Midsummer Night*, July *The Summer Heat*, August *The Harvest Fair*, September *The Back-To-School Special*, October *The Haunted Month*, November *The Feast That Ate Everything*, December *The Winter Festival*.
- **Procedural quests:** a generator that assembles a title, a cast and five beats from interchangeable parts, with realm-specific adjectives, nouns and beats. Millions of combinations; about a third of all new quests are generated.
- **28 Twists** — once-per-save story turns that **change your hero's DNA or world**:
- The Pet Is A Dragon
- The Nemesis Confession
- The Patron's Bankruptcy
- The Catchphrase Trademark
- The Secret Escapes
- The Flaw Gets A Fan Club
- The Origin Revised
- The Title Upgrade
- The Obsession Explained
- The Nemesis Upgrade
- The Long-Lost Aunt
- The Evil Twin
- The Fan Surge
- The System Glitch
- The Companion's Secret Past
- The Wrong Hero
- The Sword Files A Complaint
- The Hero Is Famous Somewhere Else
- The Rival Hero Drops By
- The Bard Writes The Wrong Song
- The Map Was Upside Down
- The Tavern Remembers Everything
- The Time Loop That Wasn't
- The Hero's Biography Gets Published
- The Moon Is Mad At You
- The Hero's Hat Becomes Sentient
- The Merchant Is Immortal
- The Audience Takes A Vote

You can pay a bard (Legacy tab) to start one immediately. The profile shows your current side quest.

### 9.4 Callbacks and memories
Completed quests and big moments (boss kills, statues…) are stored as short **memories** ("the day Dave defeated Gary") that later lines can refer back to, so the story seems to remember.

### 9.5 Chronicle and System voice
Between the plots, the feed runs a steady stream of: realm-specific observations, level intros, NPC encounters, shop and tavern scenes, system announcements ("THE SYSTEM IS WATCHING. THE SYSTEM IS ALSO ON HOLD."), sponsor pop-ups, audience chat reactions, and dilemma scenes with three outcomes each.

---

## 10. Trophies, bestiary, errands, daily gift

Open the **☰ menu** for five tabs:
- **TROPHIES:** 51+ achievements with coin rewards, from *Pocket Change* and *Coin Goblin* (coin totals) to *Boss Landlord*, *Globetrotter*, *Brimstone Tourist*, *Edge of Everything*, *The Unfinished Draft*, *Fan Favorite*, *Streaming Sensation*, *Subplot Enthusiast* and *Entire Anthology* (side quests), *Mythic Mugger*, *Level Century*, *Suspiciously Dedicated* (daily streaks), *Let It Ride* and more.
- **BESTIARY:** the three foes of each realm; defeat them to fill it in.
- **ERRANDS:** send the hero off on a timed errand (one at a time), then come back later to claim coins and gear. The *Odd Jobs* and *Professional Errand Person* trophies count them.
- **CODEX:** a collection page for every **Legendary** item in the game; you fill it in as you find them.
- **AUTO:** the gnomes and save backup (below).

**Daily gift:** the first visit each day gives a coin gift; consecutive days build a **streak multiplier** that maxes out at 7 days.

---

## 11. Automation (the gnomes)

In **☰ → AUTO**:
- **Shop Gnome:** buys upgrades automatically (never spending more than 70% of your coins), picking good value across all upgrades.
- **Chest Gnome:** opens one chosen chest tier whenever it costs less than 45% of your coins.
- **Save backup:** COPY gives you a text code of your whole save; LOAD restores from one.

The gnomes work offline too.

---

## 12. Saving, backups and privacy

- Your hero is auto-saved in the browser's `localStorage` (key `heroically_idle_v4`).
- **Backup:** ☰ → AUTO → **COPY** copies a save code (base64 JSON); **LOAD** pastes one back. Use this to move between devices or addresses, or before clearing browser data.
- **NEW GAME** (in the menu) erases the hero after a confirmation tap.
- The game makes **no network requests** and uses **no analytics, accounts or ads**. Sound is generated in the browser with the Web Audio API; there are no audio files. The only other stored value is your sound on/off choice.
- Clearing site data or using private browsing removes the save.

---

## 13. Balance and pacing

Design target: **about two years of content at roughly an hour a day**.

Figures from long-run simulations (always-on shop and chest gnomes, offline pace):

| Measure | Result |
|---|---|
| Time to clear one realm (300 levels) | about 66–74 hours |
| All 13 realms | about 900 hours (≈2.5 years at 1 h/day; faster with active play and speed-ups) |
| Hero level after 400 hours | ~47 |
| Coin hoard after 400 hours | about 1–2 hours of income (sinks keep up) |
| Boss fight length | 15–76 seconds across the tested realms |

Key pacing constants (all in `source/src`):
- `LVR = 300` levels per realm; `stageLen() = 520`; `RUN_V = 52`.
- Offline rate = `0.12 × k × RUN_V × (1 + momentum cap × 0.5)`.
- Enemy HP clamp: between `3.2 × power × hm` and `12 × power × hm` (hm: champion 3, guardian 7, realm boss 16).
- Chest price: `40 × (1 + 0.4·level) × tier × max(1, coin value / 4)`.
- Legacy prices are multiples of your *base* income (`ENDC`): bard 3,000×, statue 20,000×, festival 900×, endless upgrades 250–350× × 1.2^level.

Progression is time-gated by design (enemy strength follows your power), so the economy can't be rushed by hoarding coins; coins buy convenience, fame and stories.

---

## 14. Technical notes & project layout

```
heroically-idle/
├─ index.html          ← the game (self-contained; this is all you need to host)
├─ README.md           ← this file
├─ docs/screenshots/   ← screenshots used here
├─ source/
│  ├─ build.py         ← stitches src/*.js into index.html  (python3 build.py)
│  └─ src/
│     ├─ template.html ← HTML, CSS, core engine, __game debug export
│     ├─ c1..c4.js     ← word banks & dilemma scenes; realm data
│     ├─ c5.js         ← running, fighting, upgrades, shop
│     ├─ c6.js         ← pixel art: hero, gear, icons
│     ├─ c7.js         ← title screen, trophies, menu, automation
│     ├─ c8a–c.js      ← chronicle, realm and level flavor text
│     ├─ c9.js         ← momentum, c10.js bosses, c11.js scenery
│     ├─ c12.js        ← the Saga (stage & level beats)
│     ├─ c13.js        ← sound effects (Web Audio)
│     ├─ c14.js        ← Story DNA, side-quest engine
│     ├─ c15.js        ← realm-specific quests (3 per realm)
│     ├─ c16.js        ← Grand Plot and twists
│     ├─ c17.js        ← calendar quests, extra quests, procedural plots, more upgrades
│     ├─ c18.js        ← realm procedural parts and extra twists
│     ├─ c19.js        ← late-game sinks (endless upgrades, Legacy tab)
│     └─ c20.js        ← Make It Yours, pronouns, Legend Card
└─ tests/              ← development test scripts (Playwright; see section 15)
```

- Pure HTML + CSS + JavaScript, ~620 KB, no dependencies. The runway is a pixel-art canvas, redrawn with `requestAnimationFrame`; all sprites (hero, gear, foes, bosses, icons, logo) are drawn procedurally in code, no image files.
- To edit: change files in `source/src/`, then run `python3 build.py` in `source/`. It rewrites `../index.html`.
- `window.__game` exposes internals for testing in the browser console.
- Layout: portrait-first, max width 560 px centered on large screens; landscape and short screens have their own title layout.

---

## 15. Testing

The `tests/` scripts are Playwright + Chrome tests used during development. They contain absolute paths from the build machine (`/home/claude/...`); change those to run them. The suites cover:
- **Boss flow** across early, middle and late realms (`t23.py`);
- **Placeholder/grammar scans** over tens of thousands of generated lines (`tx.py`, `tstory.py`, `tnew.py`, `tgram.py`), with no unresolved `{slot}`, `undefined`, `NaN` or `[object]`;
- **Repetition** (`trep.py`), **pronouns** (`tpro.py`), **legacy shop** (`tleg.py`), **saves and backup** (`tsave.py`), **companions** (`tcomp.py`, `tc2.py`), **offline gnomes** (`tg.py`), **UI/menu** (`t20.py`), **sound button** (`tsnd.py`);
- **Long-run balance simulation** (`t30.py`, `tb.py`).

Results at release: all suites pass; 90 side-quest/twist/calendar definitions run end to end; 3,900 procedural quests and ~28,700 generated lines clean; 2,000/2,000 unique heroes.

---

## 16. Known gaps (honest list)

- **Not tested on a real phone or in Safari.** All testing was done in desktop Chrome at phone-sized viewports. Please try it on your phone before publishing: especially sound (iOS needs a first tap to unlock audio), the Legend Card Share button, and long-press save.
- **Pronouns are best-effort.** The story is written with "he" for the hero. For she/they the game rewrites pronouns when it is confident, and skips lines where another character is involved, so some lines (roughly 7%) still say "he", and "they" verb agreement is simple. Words such as "guy" or "man" for the hero aren't changed.
- **Humor is subjective.** There are thousands of lines; some will land better than others.
- **No-repeat memory resets on reload.** Within a session nothing repeats within ~150 lines; across days, familiar lines return, as they must.
- **Older upgrades max out early (about realm 5).** The six newer upgrades and the three endless upgrades keep gold useful afterwards.
- **Late-game numbers** (Legacy prices, statue/festival strength) were tuned by simulation, not by real long-term players.
- **Calendar quests use the device date**, so a player can change their clock to see another month's quest.
- **Single device, single hero.** Cloud saves aren't included; use Save backup to move heroes.
- **Accessibility:** a text-size toggle exists, but there's no screen-reader work and no reduced-motion option.

---

## 17. Version history

| Build | Highlights |
|---|---|
| v1–v11 | Core runner + feed + dilemmas, gear, chests, shop, 13 realms, bosses, offline catch-up, profile, title screen |
| v12 | Fight-length balance clamp |
| v13 | Title-screen fix, sound effects, saga variety, trophies, offline gnomes |
| v14 | Rare foe art, hero gear flourishes, offline pacing fix |
| v15 | Story Engine: Story DNA, side quests, memories |
| v16 | Longer companion stays, pet sprite, save backup, offline story beats |
| v17 | Realm side quests, Grand Plot, 16 twists, calendar quests, 6 new upgrades, procedural plots |
| v18 | Realm procedural parts, 12 more twists, balance for long play |
| v19 | No-repeat system, grammar fixes, consistent hero pronouns |
| v20 | Late-game sinks: endless upgrades, Legacy tab, big-number formatting, release metadata |
| **v21 (1.0)** | **Make It Yours (names, pronouns, friend), Legend Card, September quest fix, release package** |

---

*Made for the joy of watching a very small hero make very large mistakes.*
