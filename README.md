# Ragdoll Rigger 🪢

A prototype for a physics puzzle-platformer: **move a floppy, 3D-style ragdoll
through a 2D world** — and *engineer the aids* (swing-ropes, later bridges) that
let it cross terrain it can't cross on its own.

It fuses two genres that rarely meet:

- **Ragdoll locomotion** — the reactive, comedic dexterity of *Gang Beasts* /
  *Human Fall Flat*. The doll is an active ragdoll: joints have "muscle tone"
  (springy motors) so it tries to hold itself upright but gets thrown around by
  real physics.
- **Engineering / construction** — the deliberate planning of *Poly Bridge* /
  *Bridge Constructor*. You rig the crossing, then physically pull it off.

The loop this prototype demonstrates: **there's a canyon → build a swing-rope
over it → grab the rope and swing the doll across to the flag.**

## Run it

The game loads the [Rapier2D](https://rapier.rs/) physics engine as an ES
module, which browsers block over `file://`. Serve it from a local server:

```bash
# from the repo root
python3 -m http.server 8000
# then open http://localhost:8000
```

(Any static server works — `npx serve`, `php -S localhost:8000`, etc.)

## Controls

| Action | Keys |
| --- | --- |
| Move / lean | `A` `D` or `←` `→` |
| Jump | `W` / `↑` / `Space` |
| Grab rope or ledge (both hands) | hold `J` or left-click |
| Toggle **Build mode** | `B` |
| Place a swing-rope (in build mode) | click along the highlighted overhang |
| Reset the doll | `R` |
| Undo last rope | `Backspace` |

**Swing tip:** grab a rope, then tap `A`/`D` to pump the swing like a real
pendulum, and let go at the top of the arc to launch.

## What's in this prototype

- ✅ Active ragdoll (torso, head, jointed arms + legs) constrained to a 2D plane
- ✅ Physics-driven locomotion, jumping, and self-uprighting balance
- ✅ **Grab verb** — hands dynamically attach to ropes/ledges and release
- ✅ Rope simulation (jointed segment chain) you can grab and swing on
- ✅ **Build mode** — place swing-ropes along an overhang: the two-loop fusion
- ✅ A canyon, a goal flag, fall/win/reset handling, follow camera

## Tech

- **Physics:** Rapier2D (Rust → WASM), loaded via CDN — fast, stable joints.
- **Rendering:** plain Canvas 2D (no build step, single file).
- Everything lives in [`index.html`](./index.html).

## Design notes & roadmap

This is deliberately a "feel the core loop" build. Deciding it's fun *here* is
cheaper than committing to a full engine first. Natural next milestones:

1. **3D visual skin** — port to Three.js + Rapier3D with the body Z-locked to
   the plane, so limbs flop in 3D while gameplay stays 2D. (Physics stays the
   same shape; only rendering + a depth axis change.)
2. **Bridge building** — planks as rigid bodies with joint *break forces*, so
   overloaded structures snap (the *Poly Bridge* structural-stress puzzle).
3. **Better active-ragdoll walking** — PD-controlled stepping gait instead of
   applied body force; this is the biggest tuning investment.
4. **Level system + budgets** — limited rope/plank inventory per level, valid
   anchor surfaces, par times.
5. **Juice** — grab/impact SFX, momentum trails, ragdoll face reactions.

The two loops (calm planning vs. tense dexterity) currently interleave as
**build-then-play**; a "pause-to-build mid-run" mode is a good later layer.
