# darktide-mods-perfect-keystones
Perfect Keystones adds a small ring around the crosshair that tracks your class **keystone**: how long a timed effect still runs, how many stacks you hold and when the next one will expire, how far a resource has built up before an activation, Until Death's protection and cooldown, and Lone Wolf's time to the next Blitz charge. The game's buff bar shows the same effects as small icons at the edge of the screen; the ring puts them where you are already looking.

It covers all 25 keystones of the seven classes. 23 of them draw a ring; Go Get 'Em! and Unleashed Brutality are permanent companion specialisations with nothing to count down, so they deliberately draw nothing.

The mod is purely visual. It never changes buffs, procs, cooldowns or inputs, and it hooks no game function: it only reads your character's buffs, its talent resource and inventory, and your Blitz charges.

The ring style follows [Perfect Thrust](https://github.com/LucLeto/darktide-mods-perfect-thrust), and both mods can run at the same time (see [Perfect Thrust](#perfect-thrust)).

## Display

The rings sit around the crosshair, made of the same square segments as Perfect Thrust's ring. The first segment is at 12 o'clock and segments light clockwise from there; a timed effect starts as a full ring that empties **counter-clockwise** back toward 12 o'clock as its time runs out. Colours shown are the defaults (see [Colors](#colors)):

| What the keystone shows | Ring |
| --- | --- |
| Timed active effect | Full purple ring that empties counter-clockwise; it turns red-orange for the last 25 % of the duration, and fills again when the effect is refreshed |
| Stacks that expire one at a time | Each held stack fills its share of the ring; only the share of the **next** stack to expire turns from purple toward red-orange as its timer runs down. The filled area only shrinks when a stack is actually lost |
| Stacks with one shared timer | Each held stack fills its share; as the shared timer runs down, the filled segments turn red-orange **one after another**, from the end of the filled area back toward 12 o'clock. When the last one has turned, the stacks are lost. A refresh turns them purple again; with more stacks, more segments are lit and they turn faster |
| Stacks or resources without a timer | Each held stack fills its share, in whole steps |
| Cooldown, next Blitz charge | Light blue ring that empties counter-clockwise |
| Stack being restored (Feel No Pain, Focus Target!) | The next stack's share fills in light blue over the restore interval and turns purple when the stack returns |
| Nothing to show | Hidden |

A ring pulses briefly when an effect activates, is refreshed, switches to its cooldown or reaches its maximum stacks. Optionally it can also pulse on every stack change, and show the stack count right of the rings and the remaining seconds left of them, each label movable with its own offsets. The inner ring's labels sit just below the rings' centre line, the outer ring's just above it.

### Two rings

Two classes can take a second keystone next to their main one. Each keystone then has its own fixed ring:

| Class | Inner ring | Thin outer ring |
| --- | --- | --- |
| Zealot | Blazing Piety, Martyrdom or Inexorable Judgement | Until Death |
| Arbitrator | Execution Order, Terminus Warrant or Forceful | Lone Wolf |

Both rings are tracked and drawn independently: one never hides, replaces or resets the other, and a ring that hides leaves the other exactly where it is. Every other class has one keystone and uses the inner ring only.

Until Death and Lone Wolf each have a `Ring position` setting. Set to `Inner ring`, the second keystone swaps places with the main keystone, which then uses the outer ring. Each ring keeps its own size, thickness and label position.

## What it tracks

Every value is read from the game while you play: stack counts, maximums and durations come from your actual buffs, so talent upgrades that raise a maximum or lengthen a timer are followed without a per-talent list. A keystone only shows a ring while it is part of your build, and only when its checkbox is on. The rings work in missions, the Psykhanium and the training grounds; in the Mourningstar, where your talents are active as well, they stay hidden.

### Tracking models

| Model | Meaning | Keystones |
| --- | --- | --- |
| Timed buff | One buff with a remaining time; a refresh restarts it | Execution Order |
| Individual stack decay | Stacks expire one at a time on a timer that restarts after every gain or loss | Marksman's Focus, Warp Siphon, Disrupt Destiny, Forceful, Chemical Dependency, Redline Capacitors |
| Shared refreshable timer | All stacks expire together when their common timer runs out; gaining a stack refreshes it | Heavy Hitter, Burst Limiter Override, Vulture's Mark |
| Discrete stacks | Held stacks or resources without a timer | Focus Target!, Martyrdom, Feel No Pain, Empowered Psionics, Flensing Protocols |
| Hybrid | A resource builds up; once the timed active buff runs, its remaining time takes priority and the rebuilt resource stays hidden until it ends | Weapons Specialist, Blazing Piety, Inexorable Judgement, Terminus Warrant, Adrenaline Frenzy, Power Overload |
| Proc cooldown | A protection countdown, then its cooldown | Until Death |
| Next-charge replenishment | The time until the next automatic Blitz charge | Lone Wolf |
| No ring | Permanent specialisation with nothing to count | Go Get 'Em!, Unleashed Brutality |

### Keystones

| Class | Keystone | The ring shows | Visible |
| --- | --- | --- | --- |
| Veteran | Marksman's Focus | Held stacks (10, or 15 with the upgrade); the next stack's share warns before it decays | With stacks |
| Veteran | Focus Target! | Held tagging stacks (4, or 6 with the upgrade), in whole steps; the next stack's share fills in light blue over its 1.5 s build-up, regained stacks sweep in | Below the maximum, including 0 (or always) |
| Veteran | Weapons Specialist | The active buff of the wielded weapon's remaining time; otherwise the resources stored for your next weapon switch; new resources sweep in | With a buff or resources (or only below a full pool) |
| Zealot | Until Death | Outer ring: the protection's countdown after it saved you (8 s), or after your combat ability with the Zealous Pilgrim modifier (4 s), then the cooldown in the cooldown colour | During a protection, and until it is ready again |
| Zealot | Blazing Piety | Fury (25) before activation; while the personal buff runs, its remaining time | With Fury or the buff |
| Zealot | Martyrdom | Stacks from missing health segments against the keystone's maximum of 5, so a full ring always means full strength; new stacks sweep in | With stacks (or only below the maximum) |
| Zealot | Inexorable Judgement | Held Momentum (20); while active, its remaining time (6 s, or 10 s with the upgrade), however many stacks it was activated with | With Momentum or while active |
| Ogryn | Heavy Hitter | Held stacks (8); the filled segments turn red-orange one by one as the shared timer runs down | With stacks |
| Ogryn | Burst Limiter Override | Held damage stacks from ranged kills (10); the filled segments turn red-orange one by one as the shared timer runs down | With stacks |
| Ogryn | Feel No Pain | Strength stacks (10) in whole steps; the next stack's share fills in light blue over the 2 s restore interval, regained stacks sweep in | Below the maximum, including 0 (or always) |
| Psyker | Warp Siphon | Held Warp Charges (4, or 6 with the upgrade); the next charge's share warns before it decays | With charges |
| Psyker | Disrupt Destiny | Held stacks (15, or 25 with the upgrade; 5 s or 10 s per stack); the next stack's share warns | With stacks |
| Psyker | Empowered Psionics | Stored empowered Blitz charges (1, or 3 with the upgrade) | With charges (optionally only with the Blitz in hand) |
| Arbitrator | Execution Order | The personal buff's remaining time; marks, the Cyber-Mastiff's buff and permanent stacks do not count | While the buff runs |
| Arbitrator | Terminus Warrant | The active melee or ranged effect's remaining time; otherwise the stacks stored for your next weapon switch | With an effect or stacks |
| Arbitrator | Forceful | Held stacks (10); the next stack's share warns before it decays | With stacks |
| Arbitrator | Lone Wolf | Outer ring: the time until your next automatic Blitz charge (45 s for grenades, 90 s for the Shock Mine) | While charges are missing |
| Arbitrator | Go Get 'Em!, Unleashed Brutality | Nothing, by design | Never |
| Hive Scum | Vulture's Mark | Held stacks (3); the filled segments turn red-orange one by one as the shared timer (8 s, or 12 s with the upgrade) runs down | With stacks |
| Hive Scum | Adrenaline Frenzy | Build-up stacks (30) before Frenzy; while Frenzy runs, its remaining time | With stacks or Frenzy |
| Hive Scum | Chemical Dependency | Held stacks (3 at 90 s each, or 4 at 60 s with the upgrade); the next stack's share warns | With stacks |
| Skitarius | Redline Capacitors | Held stacks (4, or 5 with the upgrade); the next stack's share warns before it decays | With stacks |
| Skitarius | Power Overload | Your charge toward the next overload (30); while the Power Overload buff is on you, its remaining time, also when an ally triggered it | With charge or the buff |
| Skitarius | Flensing Protocols | Strength stacks (6, or 8 with the upgrade) in whole steps; regained stacks sweep in | Below the maximum, including 0 (or always) |

The numbers in brackets are the values of game version 1.13.2, for orientation; the mod reads them from the game.

* **Hybrid keystones show one thing at a time.** While the active buff runs, its countdown takes the ring, and resources you rebuild meanwhile stay hidden, also from the stack label. When it ends, the ring switches straight to the resource you hold now.
* **Weapons Specialist and Terminus Warrant** show the pool for your *next* switch: with your melee weapon out, the resources that the switch to ranged will use, and the other way round. With the Blitz or an item in hand, no pool is shown.
* **Weapons Specialist and Martyrdom** have the same `Visibility` setting, but default to `Always` and stay hidden at zero in both modes: `Always` shows them from the first stack up to the maximum, `Below maximum` hides them again a second after the maximum is reached. Weapons Specialist's active countdown is shown either way.
* **Feel No Pain, Flensing Protocols and Focus Target!** stay out of the way at their maximum and appear as soon as you lose a stack. At zero the ring stays visible, empty. Regained stacks sweep in over 0.3 s, and when the maximum returns the full ring stays visible for a second before it hides. Their `Visibility` setting can show them at the maximum too.
* **Refill of Feel No Pain and Focus Target!** Feel No Pain restores one stack 2 s after your last lost and your last restored stack, and keeps restarting that timer while you are knocked down; Focus Target! gains one stack 1.5 s after its last change, such as a gained stack or a tag that used your stacks. Your client is not sent these timers, so the mod rebuilds them from the losses and gains it sees, which arrive with the same delay as the new stack. The next stack's share fills in light blue over those 2 s, the remaining seconds label counts down to it, and the share turns purple when the stack actually arrives. Until the mod has seen a stack come or go, for example right after enabling it, no refill is shown.
* **Until Death** is hidden while it is ready. With `Show cooldown` off, it hides as soon as the protection ends.
* **Zealous Pilgrim**, the Until Death modifier, grants a shorter protection when you use your combat ability (with Shroudfield when its stealth ends, with Chorus when the channel ends). It is a separate buff that does not start Until Death's cooldown, so it shows as its own purple countdown, also while the cooldown runs, and the cooldown display continues afterwards. If both protections run at once, the ring shows the one with more time left.

<details>
<summary>Buff templates (for modders)</summary>

A keystone counts as selected while its controller or passive buff is present, so talents disabled by a game mode and talent changes are followed as the game applies them. Your buffs are scanned once when you join a mission or respawn; after that the game's `event_player_buff_added` and `event_player_buff_removed` events, which the game's own buff bar listens to as well, keep the watched buffs current, so nothing is scanned per frame and no game function is hooked. Counts come from `visual_stack_count()`, which already handles talent resources, parent buffs, stack offsets and custom counts. Timers are `start_time() + duration()` against `FixedFrame.get_latest_fixed_time()`, the clock the buffs use; `duration()` includes extra duration a talent added to the instance.

| Keystone | Selected by | Read | Maximum |
| --- | --- | --- | --- |
| Marksman's Focus | `veteran_snipers_focus` | `veteran_snipers_focus_stat_buff`, `_increased_stacks` | `max_stacks()` |
| Focus Target! | `veteran_improved_tag` | same buff (`talent_resource.current_resource`) | `template_data().max_stacks` |
| Weapons Specialist | `veteran_weapon_switch_passive_buff` | active: `veteran_weapon_switch_ranged_buff`, `_melee_buff`; pool: `veteran_weapon_switch_ranged_visual` (melee wielded), `_melee_visual` (ranged wielded) | `max_stacks()` |
| Until Death | `zealot_resist_death` | same buff: `active_start_time()`, `_active_duration()`, `_cooldown_duration()`; Zealous Pilgrim's protection: `zealot_resist_death_temp` | - |
| Blazing Piety | `zealot_fanatic_rage` | active: `zealot_fanatic_rage_buff`; resource: same buff | `talent_resource.max_resource` |
| Martyrdom | `zealot_martyrdom_base` | same buff | `template_data().max_stacks` |
| Inexorable Judgement | `zealot_quickness_passive` | active: `zealot_quickness_active`, `_increased_duration`; resource: same buff | child template `max_stacks` |
| Heavy Hitter | `ogryn_passive_heavy_hitter` | `ogryn_heavy_hitter_damage_effect` | `max_stacks()` |
| Burst Limiter Override | `ogryn_blo_new_passive` | `ogryn_blo_stacking_buff` | `max_stacks()` |
| Feel No Pain | `ogryn_carapace_armor_parent` | same buff | child template `max_stacks` |
| Warp Siphon | `psyker_passive_souls_from_elite_kills` | `psyker_souls`, `_increased_max_stacks` | `max_stacks()` |
| Disrupt Destiny | `psyker_marked_enemies_passive` | `psyker_marked_enemies_passive_bonus_stacking`, `_increased_stacks`, `_increased_duration` | `max_stacks()` |
| Empowered Psionics | `psyker_empowered_grenades_passive`, `_improved` | `psyker_empowered_grenades_passive_visual_buff`, `_increased` | `template_data().max_stacks` |
| Execution Order | `adamant_execution_order` | `adamant_execution_order_buff` | - |
| Terminus Warrant | `adamant_terminus_warrant` | active: `adamant_terminus_warrant_melee_stat_buff`, `_ranged_stat_buff` (and `_improved`); pool: `adamant_terminus_warrant_ranged` (melee wielded), `_melee` (ranged wielded) | `max_stacks()` |
| Forceful | `adamant_forceful` | `adamant_forceful_stacks` | `max_stacks()` |
| Lone Wolf | `adamant_grenade_replenishment` | same buff: `template_data().next_grenade_t`, `.cooldown`; ability extension `missing_ability_charges("grenade_ability")` | - |
| Go Get 'Em!, Unleashed Brutality | `adamant_companion_focus_ranged`, `adamant_companion_focus_elite` | debug output only | - |
| Vulture's Mark | `broker_keystone_vultures_mark_on_kill` | `vultures_mark` | `max_stacks()` |
| Adrenaline Frenzy | `broker_keystone_adrenaline_junkie` | active: `broker_keystone_adrenaline_junkie_proc`; resource: `_stack` | `max_stacks()` |
| Chemical Dependency | `broker_keystone_chemical_dependency` | `broker_keystone_chemical_dependency_stack` | `max_stacks_cap()` |
| Redline Capacitors | `cryptic_redline` | `cryptic_redline_stack` | `max_stacks_cap()` |
| Power Overload | `cryptic_overload_keystone` | active: `cryptic_overload_keystone_allies_buff`; resource: `cryptic_overload_keystone_stack` | `max_stacks_cap()` |
| Flensing Protocols | `cryptic_dissector` | `cryptic_dissector_stack` | `max_stacks_cap()` |

The renderer also supports a progressive style for stacks that expire one at a time, where the next stack's share drains instead of changing colour (`decay_style = "progressive"` in the registry). Every keystone above uses the stable style, so the style has no setting.

</details>

## Settings

`Ring` group:

| Setting | Default | Range / options |
| --- | --- | --- |
| Follow crosshair | On | Moves the rings with the crosshair as camera sway, weapon sway and recoil move it; the offsets then count from the crosshair. Off keeps the rings at the screen centre |
| Ring size | 40 | 12-150 px radius of the inner ring at 1080p |
| Ring thickness | 3 | 1-10 px per inner ring segment at 1080p |
| Outer ring gap | 5 | 1-30 px between the inner and the outer ring's segments at 1080p |
| Outer ring thickness | 2 | 1-10 px per outer ring segment at 1080p |
| Horizontal offset | 0 | -960 to 960 px from the screen centre, or from the crosshair with `Follow crosshair` on |
| Vertical offset | 0 | -540 to 540 px from the screen centre, or from the crosshair with `Follow crosshair` on |
| Pulse | On | Briefly enlarges a ring's segments on activation, refresh, the switch to the cooldown and reaching the maximum stacks |
| Pulse on every stack change | Off | With `Pulse` on, also pulses whenever a stack is gained or lost |
| Show stack count | Off | Number of held stacks, right of the rings |
| &nbsp;&nbsp;Horizontal offset, Vertical offset | 0, 0 | -200 to 200 px at 1080p from the stack count's default place; nested under `Show stack count` |
| Show remaining seconds | Off | Seconds of the ring's timer, left of the rings, with tenths below 10 s |
| &nbsp;&nbsp;Horizontal offset, Vertical offset | 0, 0 | -200 to 200 px at 1080p from the remaining seconds' default place; nested under `Show remaining seconds` |

`Appearance` group:

| Setting | Default | Range / options |
| --- | --- | --- |
| Active color | RGB `170, 90, 255`, alpha `235` (purple) | Lit segments: stacks, resources, active effects |
| Unfilled segment color | RGB `70, 82, 86`, alpha `80` (faint grey) | Segments that are not lit |
| Expiring color | RGB `255, 90, 70`, alpha `235` (red-orange) | What an expiring stack turns toward, and an active effect's last moments |
| Cooldown color | RGB `110, 175, 235`, alpha `220` (light blue) | Until Death's cooldown, Lone Wolf's next Blitz charge and Feel No Pain's stack being restored |
| Near-expiry warning | 25 % | 0-100 % of an active effect's duration at whose end the ring turns the expiring colour; 0 turns it off. Cooldowns are not affected |

One group per class, with a checkbox for each keystone that draws a ring, all on by default:

| Group | Keystones | Extra settings |
| --- | --- | --- |
| Veteran | Marksman's Focus, Focus Target!, Weapons Specialist | `Visibility` for Focus Target! (default Below maximum) and Weapons Specialist (default Always) |
| Zealot | Until Death, Blazing Piety, Martyrdom, Inexorable Judgement | `Show cooldown` (on) and `Ring position` (default Outer ring) for Until Death, `Visibility` (default Always) for Martyrdom |
| Ogryn | Heavy Hitter, Burst Limiter Override, Feel No Pain | `Visibility` (Below maximum / Always) for Feel No Pain |
| Psyker | Warp Siphon, Disrupt Destiny, Empowered Psionics | `Only show with Blitz selected` (off) for Empowered Psionics |
| Arbitrator | Execution Order, Terminus Warrant, Forceful, Lone Wolf | `Ring position` (default Outer ring) for Lone Wolf |
| Hive Scum | Vulture's Mark, Adrenaline Frenzy, Chemical Dependency | |
| Skitarius | Redline Capacitors, Power Overload, Flensing Protocols | `Visibility` (Below maximum / Always) for Flensing Protocols |

The extra settings sit indented under the checkbox of the keystone they belong to, and are hidden while that keystone is switched off. The keystone and class names in these groups come from the game's own localization, so they match your talent tree in your game language.

`Miscellaneous` group:

| Setting | Default | Range / options |
| --- | --- | --- |
| Debug output | Off | Writes detected keystones and phase changes to the game's log file (see [Debug output](#debug-output)) |

Every setting applies immediately. Changing a keystone's settings only changes what is shown: tracking reads the game's state afresh every frame, so toggling a checkbox, the cooldown display or a visibility option never resets, pauses or consumes anything. The rings scale with your resolution like the game's own crosshair and ignore the HUD scale option. Disabling the mod through the standard mod toggle hides the rings and stops all tracking.

### Colors

Each colour has its own picker with an alpha channel, from 0 (invisible) to 255 (fully opaque). The expiring colour is reached gradually. For stacks that expire one at a time, the next stack's share moves from the active colour toward it in 32 steps over its timer, so it is fully red-orange just as that stack expires. For stacks with a shared timer, the filled segments switch to it one after another at an even pace, so the whole filled area has turned just as the stacks expire. The near-expiry warning of a timed active effect switches to it in one step.

## Perfect Thrust

Perfect Keystones and Perfect Thrust are separate mods and can be enabled together without changes to either. Perfect Thrust's ring has a 32 px radius by default; Perfect Keystones' inner ring defaults to 40 px and its outer ring to about 48 px, so all rings fit around the crosshair without touching, also while they pulse. If you enlarge Perfect Thrust's ring, raise `Ring size` accordingly, or move one of them with the offsets.

## Debug output

With `Debug output` enabled, the mod writes what it detects to the game's log file. The log files are in `%APPDATA%\Fatshark\Darktide\console_logs`; the lines are DMF info messages, `[MOD][PerfectKeystones][INFO] [Perfect Keystones] ...`:

```text
[Perfect Keystones] bound to a cryptic, watching 7 buff templates
[Perfect Keystones] redline_capacitors (cryptic_redline) selected
[Perfect Keystones] redline_capacitors: hidden -> held (1/4, timer 12.00/12.00s)
[Perfect Keystones] redline_capacitors: held -> hidden
[Perfect Keystones] power_overload: building -> active (8.00/8.00s)
```

A line is written when a keystone becomes selected or unselected and whenever its ring's phase changes: `active` (a timed effect), `building` (a resource before an activation), `held` (stacks), `cooldown`, `replenishing` (Lone Wolf) or `hidden`. Go Get 'Em! and Unleashed Brutality log `selected, no ring by design`.

## Limitations

* **Timers on a multiplayer client follow the server's messages.** Most keystone buffs are granted by the server; your client starts their timers when the message arrives, so in multiplayer a countdown can run up to one ping behind the server, and a stack can disappear a moment after the ring reached its warning colour. Refreshes the server sends correct the timer. In solo play there is no delay.
* **Lone Wolf follows your client's own replenishment timer.** The game runs this timer on your client too, from your client's charge count; it can differ slightly from the server's in multiplayer.
* **Starting stacks of Feel No Pain and Flensing Protocols** reach your client over the network when you spawn. A missing stack only shows half a second after the effect appears, so the ring does not flash while they arrive.
* **Terminus Warrant and Weapons Specialist show no pool** while the Blitz or an item is in hand, as the next switch is not known then.
* **Expired buffs vanish when the server says so.** A buff whose timer ran out on your client stays at an empty ring until the server removes it, usually within a ping.
* **No Custom HUD integration.** Position the rings with the offset settings.
* The mod reads a few internal fields of the game's buffs. A game update that renames them hides the affected ring until the mod is updated; it does not affect gameplay.

## Credits & Sources

Keystone names and values follow the game's talent definitions and the excellent guides by [kuli](https://steamcommunity.com/id/kulii):

* [\[1.13.x\] Veteran Talents & Mechanics](https://steamcommunity.com/sharedfiles/filedetails/?id=3094038976)
* [\[1.13.x\] Zealot Talents & Mechanics](https://steamcommunity.com/sharedfiles/filedetails/?id=3088553235)
* [\[1.13.x\] Ogryn Talents & Mechanics](https://steamcommunity.com/sharedfiles/filedetails/?id=3094034467)
* [\[1.13.x\] Psyker Talents & Mechanics](https://steamcommunity.com/sharedfiles/filedetails/?id=3094028505)
* [\[1.13.x\] Arbitrator Talents & Mechanics](https://steamcommunity.com/sharedfiles/filedetails/?id=3472722314)
* [\[1.13.x\] Hive Scum Talents & Mechanics](https://steamcommunity.com/sharedfiles/filedetails/?id=3586590987)
* [\[1.13.x\] Skitarius Talents & Mechanics](https://steamcommunity.com/sharedfiles/filedetails/?id=3674757853)

Game mechanics were checked against [Aussiemon's Darktide source code](https://github.com/Aussiemon/Darktide-Source-Code) for version 1.13.2. The ring rendering comes from [Perfect Thrust](https://github.com/LucLeto/darktide-mods-perfect-thrust).
