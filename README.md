# HALFWORLD

*A paper-and-ink engine for making films out of worlds.*

> Halftone + HALFTRACK + world. And the diagnosis in the name: every piece of it
> is **half a world** — characters without places, places without characters,
> scores without stages, minds without bodies. The engine is the weld.

---

## 0. The one-sentence version

HALFWORLD is a small, self-contained studio in which **a character belongs to a
scene, a scene belongs to a world, every layer is plain JSON, and the same handful
of verbs — pose, walk, say, place, remember — is spoken equally by a hand on the
screen, a game loop, a wall-clock score, and an AI mind.** You compose a world of
dot-matter and performers; you shoot it into film; you branch, arrange, and publish
the film. The whole thing runs from a single HTML file with no server and no build.

Everything below is an unpacking of that sentence.

---

## 1. What it is

HALFWORLD began as a diagnosis, not a program.

There were nine separate artifacts, each beautiful and each incomplete. One knew
how to make **characters** — halftone paper puppets with a real rig, poses, hands,
faces (HALFTRACK). Two knew how to build **worlds** — type any word and a thing
exists, place it, layer it, recurse into it (Worldsmith, Atlantis). Several knew how
to run **minds and scores** — agents whispering to each other, a timecoded call you
could scrub, a planet that answers your words with bounded acts of world-change
(the SYNTHCINE family, Virentis). A last one knew how to turn all of this into
**editable film** — a branching tree of 128×96 dot-matrix frames you could cut,
fork, and print (OPERATOR).

Each had solved a real problem. None could talk to the others. A character made in
one could not walk into another. A scene composed in one could not become a shot in
the film tool. The movement was gorgeous and then it was gone the moment it became
pixels.

HALFWORLD is the claim that **these were never nine programs. They were five
contracts wearing nine costumes** — and that if you lift the contracts out and make
them canonical, the artifacts stop being rivals and become organs of one body.

So: HALFWORLD is (1) a **theory** — five contracts and two laws — and (2) a working
**studio** that implements the theory and welds to OPERATOR, the film lab, so that a
composed world becomes a branchable, publishable film.

---

## 2. The philosophy

### 2.1 Half a world

The name is honest about the method. Nothing here was invented whole. Every part is
half of something — and the work is not to build the missing half from scratch but
to find the *seam* where two halves already fit. The engine is not a monument; it is
a set of welds. This is a philosophy of **assembly**: you make new things by joining
things that already carry meaning, at the joints where they were always meant to meet.

This is why HALFWORLD is suspicious of the everything-app. The one artifact that tried
to be everything-at-once (the original HALFTRACK "rig v5") is the cautionary tale in
its own comments: it swallowed the world instead of *contracting* with it, and so it
did many things and finished none. HALFWORLD's answer is the opposite: keep each part
small and single-minded; let them cooperate by speaking shared contracts over shared
channels. **Contracts, not merges.**

### 2.2 The two laws

Everything in HALFWORLD obeys two laws. They are short because they are load-bearing.

**The law of the dot.** *All geometry bottoms out in ink dots on paper.* A mouth, a
person, a house, a planet, a whole film frame — every scale is the same substrate at
a different density. Worldsmith's sprite grids, Atlantis's point clouds, HALFTRACK's
tone screens, OPERATOR's BEFLIX frames: one material, four resolutions. Because the
material is uniform, **any renderer can draw any contract**, and any composed image
can be sampled down into the film's 128×96 grid without asking permission. The dot is
not decoration. It is the common denominator that makes the welds possible — and it
is the film's grammar. (More on that in §2.6.)

**The law of the packet.** *A mind gets exactly the powers a finger has — no more.*
When you drag a character, you emit an operation: `walk here`. When an AI directs the
scene, it emits the *same* operation, in the same schema, validated and clamped the
same way (Virentis caps its world-mind to eight changes per turn; nothing an author's
hand can't do, a model can't do either — and vice versa). The mind never reaches past
the contract to touch pixels directly. This is what keeps an AI collaborator safe and
legible: **the more powerful the director, the more important the contract.** Agency,
here, is not a special capability. It is just the ordinary verb set, held by a
different hand.

### 2.3 character ⊂ scene ⊂ world

The spine of the whole system is a containment relation that HALFTRACK stated in its
own source before any of this was unified: **a character is contained by a scene; a
scene is contained by a world.** A scene is a frozen stage — its assets, its cast and
their placements and poses, its style, a thumbnail. A world is the set of its scenes.
Navigate up and you get the atlas; navigate down and you get a performer's pose.

Crucially, every level is **plain JSON**. Not a proprietary blob, not an opaque
handle — a serializable object a human can read, an LLM can write, and a file can
carry. This is the deepest design commitment in HALFWORLD: *the world is text.* If it
can't round-trip as JSON, it isn't real here. (Virentis literally calls its planet a
"worldtext" and lets you edit it by talking to it.)

### 2.4 Assembly is sampling; narrative from abundance

HALFWORLD's theory of authorship is that **making is choosing.** You do not draw
every frame; you compose a system that *generates* abundance — a stage full of
performers, a score of beats, a mind full of responses, a world of generable words —
and then you **sample** it into a sequence. Typing a word makes a thing (Worldsmith's
promise: "type to create anything is a promise, not a menu"). Playing the score makes
motion. Shooting the stage makes frames. The author's real work is the cut: which
words, which beats, which takes, in which order.

This is why the film tool matters as much as the world tool. A world that only *runs*
is a toy; a world you can *film, arrange, and cut* is a medium. Assembly (choosing the
order) is a kind of sampling from the world's abundance — and the edit is where the
meaning lives.

### 2.5 The UI is the story engine; the onboarding is the story

HALFWORLD refuses the split between "the tool" and "the thing the tool makes." The
interface is not a neutral shell around a story engine — the interface *is* the story
engine. This is literal: in the film lab, the first time you arrive, the cast —
HERO, JUNE, BOSS — **break the fourth wall and walk the interface themselves.** They
stride to a real control, point at it, and *perform* what it does: they draw on the
board, add a frame, fork the film ("this film is a tree, not a line — watch"), press
play, open the lens and take the shot. The tutorial is a scene. The onboarding is a
short film about the machine, performed inside the machine, by the machine's own cast.

The principle underneath: **show, don't tell.** Nobody is told what "capture" means;
a character opens the lens, says "to capture just means to film whatever is in front
of it — light in, frames out," and takes the shot. The explanation and the operation
are the same gesture. A tool that has to be documented separately has already failed
its own aesthetic.

### 2.6 The medium is the message (why 128×96)

When you shoot the lush halftone stage into the film, it becomes 12,288 dots — 128×96,
ink 0–7. People's first reaction is "it looks nothing like the stage." That reaction
mistakes the medium for a loss of fidelity. **BEFLIX-128 is not a downgrade of the
stage; it is the film's native language** — the same dot-matrix lineage as the early
computer animation the whole family descends from ("MADEA GOES TO A DATA CENTER," the
BEFLIX / WYGWYL / ICARO corpus). The stage is where you *compose* at full richness;
the frame is where you *commit* to the film's grain. The honest levers are two:
frame tighter so the characters fill the dots (a real in-scene camera), or accept the
abstraction as the grammar — the way a woodcut is not a failed photograph.

So HALFWORLD holds two looks in tension on purpose: the **cream, halftone, world-radio**
skin of the stage (where you build), and the **black-and-white ICARO-PRO** skin of the
film lab (where you cut). The weld's job is to make them one surface without pretending
they're one image.

---

## 3. The five contracts

The theory reduces to five JSON contracts. Each was already proven by one of the
original artifacts; HALFWORLD's job was only to make them canonical and let anything
speak them.

| Contract | What it answers | Donor | Shape (essence) |
|---|---|---|---|
| **CAST** | *who exists* | HALFTRACK | `{ id, look:{hair,skin,garment,build,height,glasses}, face:{style,tone}, pose, facing }` — the verbs `pose / walk / face / say` |
| **STAGE** | *what exists, where* | Atlantis + Worldsmith | `{ assets:{id→{kind, dots}}, instances:[{assetId,x,z,scale,rot}], cast:[{id,x,z,pose}] }` — people are placements too |
| **SCORE** | *when it happens* | SYNTHCINE Call | `{ fps, rows:[{c, s, d, t, m:"pose=wave; walk=x,z"}] }` — **state = f(playhead)**; scrub-deterministic |
| **MIND** | *who decides* | Virentis + Whispers | `{ reply, changes:[ op ≤ 8, validated, clamped ] }` — bounded op-packets, the law of the packet |
| **MEMORY** | *what persists* | Atlantis | `{ schema:"halfworld/v1", cast, stages, scores, worlds, ledger, scenes }` — autosave + append-only ledger |

Two rules bind them. **The law of the dot** unifies every renderer under one material.
**The law of the packet** routes every actor — hand, loop, score, mind — through the
same validated verbs. That's the entire engine.

---

## 4. What it does

### 4.1 The Studio — `halfworld-studio.html`

A single-file, offline studio that runs all five contracts at once:

- **Cast.** The real HALFTRACK figure rig — eight presets (HERO, SLIM, BOSS, PROF,
  KID, NOVA, JUNE, ACE), a 40-pose vocabulary, IK arms, one-path silhouette hands,
  facing bands (front / three-quarter / profile / back), hair, glasses, beard,
  garments, props, and per-character halftone tone screens. Not a stand-in — the same
  rig the source artifacts use.
- **Stage.** A ground-plane world. Type a word and a deterministic dot-sprite appears
  (hand painters plus a seeded-archetype fallback so the vocabulary is genuinely open);
  place it, scale it, drag it, draw on it, drive it (cars and dogs move through the
  scene as timeline entities).
- **Score.** A scrubbable transport. Cue rows key characters to poses, walks, and
  lines; everything is derived from the playhead, so scrubbing is deterministic. **REC**
  turns live verbs into score rows at the playhead — the weld between live and scored.
- **Mind.** One command line is the only mouth. Everything you type — "boss laughs,"
  "the car drives to the house," "a lighthouse near the house" — becomes a bounded,
  validated op-packet. Built-in offline parser (RADIO), or point it at any OpenAI-
  compatible endpoint (local-first, like Ollama) and a model speaks the identical
  contract.
- **Memory.** `halfworld/v2` project snapshots with autosave, JSON export, an append-
  only ledger of every act, named scenes, and an EDL export (every line, timecoded).
- **Capture.** A real camera drive: MediaPipe pose + face puppeteer the selected
  character from your webcam — your shrug becomes their shrug.
- **Four render styles** for the whole scene at once — SHEET, NOLINE, GRID, MESH.
- **The FILM bridge.** The stage rasterizes to BEFLIX-128 and shoots into OPERATOR.

Embedding surface: `window.HALFWORLD` — `spawn / pose / walk / generate / say / face /
drive / play / seek / grid / shootScenes / capture`. Anything that can hold this object
can drive the whole studio.

### 4.2 The film lab — `operator-studio.html` (OPERATOR)

OPERATOR is HALFWORLD's timeline, branch tree, and press. Every frame is a 128×96
BEFLIX image. The film is a **tree, not a line**: you extend, fork alternate takes,
prune, play, and cut. Five (now six) tabs — **A · AUTHOR** (make frames: draw, write,
prompt, camera, stage), **B · BRANCH** (the tree as a map), **C · CREW** (multiplayer;
words become faces; sit in as a character), **D · DIRECT** (the light table — the film
as cards you drag to recut, an overlay that never rewrites the branch), **E · IN/OUT**
(publish: GIF, video, printable flipbook, Twee graph, EDL), and **F · STUDIO** (the
whole HALFWORLD stage, hosted).

### 4.3 The weld — how a world becomes a film

The two studios are **one surface**. Inside OPERATOR, `F · STUDIO` hosts the HALFWORLD
stage chrome-less, driven entirely by OPERATOR-native controls (cast, pose, style,
camera, shoot), over a postMessage bridge that works whether you open the files from a
server or straight off disk. Then:

- **Shoot the whole score → scenes.** The score's beats segment into named scenes;
  each frame is tagged with its scene, so DIRECT groups them as scenes and BRANCH runs
  them as batches. The movement *maps* into the film's structure instead of vanishing.
- **Shoot one section at a time.** Every timeline mark is a chip. Play a section,
  perform into it (with the camera drive on, it records a live take), and send *that
  section's clip* to the main film as its own scene. You build the film beat by beat.
- **Frame it.** A capture frame that crops to the cast, so characters fill the 128×96
  dots — the fidelity lever, and the seed of a real in-scene camera.

The result: **compose in HALFWORLD → film to BEFLIX → branch and publish in
OPERATOR**, without leaving the lab. HALFWORLD is OPERATOR's scene-camera; OPERATOR is
HALFWORLD's timeline and press.

---

## 5. How to use it

1. Open **`halfworld-index.html`** — the front door. It names every part in the
   world-radio style and links the studio, the lab, and the program theory.
2. **`halfworld-studio.html`** — build a world. Spawn cast, type words to make things,
   pose and walk them, write a score, or hand the scene to the mind. Press space to
   play the score; hit **FILM** to see the stage as a BEFLIX frame.
3. **`operator-studio.html`** — open **F · STUDIO**, build or drive your scene, then
   **SHOOT SECTION → CLIP** or **SHOOT ALL → SCENES** to send it onto the film tree.
   Fork it in **BRANCH**, arrange it in **DIRECT**, publish it in **IN/OUT**.
4. First visit to OPERATOR: the cast gives you the tour. Let them.

No install, no server required. (One caveat: the *live-lens BroadcastChannel* bus
wants a shared origin, so if you open straight from `file://`, use the hosted
**F · STUDIO** stage rather than the separate live-lens source — the stage works
everywhere via postMessage.)

---

## 6. The parts

| File | Role |
|---|---|
| `halfworld-index.html` | The front door — indexes every organ in one skin |
| `halfworld-program-theory.html` | The theory as a document: the five contracts, two laws, the build path |
| `halfworld-studio.html` | The studio — all five contracts, live, in one file |
| `operator-studio.html` | OPERATOR — the BEFLIX film lab; hosts F · STUDIO |
| `HALFWORLD-README.md` | This file |

**Lineage (the nine donors):** `halftrack-stage` (CAST), `worldsmith` + `atlantis-world-studio`
(STAGE), `synthcine-whispers` + `SYNTHCINE_CALL_BODY_RIG_REBUILT` (SCORE + MIND),
`virentis-field-icaro` (MIND / worldtext), and the three `synthcine-facade-*` engines
(the target scenes). OPERATOR descends from the BEFLIX / WYGWYL / ICARO corpus.

---

## 7. What it's for

HALFWORLD is for making **essay-films, narrative machines, and worldtexts** out of
machine abundance — pieces where the story is not written frame by frame but *grown*
from a system and then *cut* from it. It is for the person who wants a cast they can
direct, a world they can generate, a mind they can argue with, and a film they can
branch and print — all in dot-matter, all as plain text, all in one afternoon in one
file.

The clearest test of the whole idea is a piece like a train carriage full of
passengers where a moving cat triggers each one's private thought, the dialogue
branches, and the beats become scenes: passengers are **cast**, the cat is a **moving
catalyst**, the thoughts are **say** rows, the branches are **BRANCH** forks, the beats
are **SCORE sections → DIRECT scenes**. That film is not a new engine. It is this one,
plus one rule we haven't yet written: *movement triggers thought.*

---

## 8. Roadmap

The engine is real and welded; three things would make it complete.

1. **The catalyst-trigger rule** in the MIND — "when a moving thing enters another's
   radius, fire a say/pose." This is what turns *characters standing in a scene* into
   *a moving thing that makes the scene happen.* It is the soul of the narrative
   machines the engine is for.
2. **A positionable in-scene camera** — the cast-frame is the seed; make it a movable,
   keyable framing so shots can push in, hold, and cut. This is also the real fidelity
   answer.
3. **True BRANCH tier-scenes** — scenes as actual parent nodes in the film tree you can
   fork, reorder, and nest as units, so the scene mapping is structural, not just
   tagged.

---

## 9. The last word

HALFWORLD's wager is simple: **the world is text, the mind is a finger, the dot is the
only material, and the edit is the meaning.** Build a small abundant world in plain
JSON; let a hand, a loop, a score, or an AI move it with the same few verbs; shoot it
into dot-matter; and cut the film out of what you find. Not one program that does
everything, but five contracts that let many small things become one — half worlds,
welded, made whole.

*character ⊂ scene ⊂ world.*
