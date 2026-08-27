# Looking for the Mao

> **FFF Shamelessly Presents**
> *Finnish Funny Fellow*

**Play it:** https://oh6hfj.github.io/looking-for-the-mao/

---

## What it is

Three screens, and the first two are the joke.

It opens at night: a humped stone bridge under a full moon, three arches with
the moonlight showing through them, one warm lantern on the railing. Press
**Start** and *Moonlight over the Ancient Bridge* comes in while the night fades
up into a 1970s martial-arts poster — sunburst rays, halftone dots, a panda in a
fighting stance holding a ping-pong bat, and the title in the fattest condensed
caps available.

Press **Begin** and the poster irises out into the game. The music crossfades to
something considerably less dignified.

Then it is whack-a-Mao. Smack them as they rise from behind the rocks; the game
counts hits, accuracy and reaction time in milliseconds. Three that get away and
it is over, and your score is entered on the Wall of Shame among the dictators of
all time.

**Do not hit the panda.** Pandas are endangered. Hitting one halves your score,
freezes the game, and the panda has something to say about it.

## How to run it

Open `index.html`. That is all — no server, no build step, no dependencies.

Everything is inlined into that one file as data URIs: both music tracks, both
sound effects, and the webfont subsets. It makes **zero external requests**, so
it works from a USB stick or offline.

## Layout

| | |
|---|---|
| `index.html` | The game. Self-contained, ~6 MB. |
| `Moonlight over the ancient bridge short.mp3` | Opening theme, over the poster. |
| `Little Itch-Scratching Mountain Song.mp3` | Game theme. |
| `ping.mp3` | Bat impact. |
| `panda.mp3` | The panda's reprisal. |
| `inline-music.ps1` | Re-inlines the audio into `index.html`. |
| `inline-fonts.ps1` | Re-inlines the webfont subsets. |

`index.html` is generated but committed on purpose — it is the deliverable, and
GitHub Pages serves it directly.

### Swapping an asset

```powershell
./inline-music.ps1        # opening theme, game theme, ping.* and panda.*
./inline-fonts.ps1        # only if the typefaces change
```

The music script matches the opening theme by name and prefers the shorter cut,
so it can never be confused with the game's track.

## Under the hood

No engine, no framework, no bundler. One HTML file with a `<canvas>`.

- **Every picture is drawn at runtime.** The bridge, the poster scene and the
  three inset vignettes are canvas paths; the sunburst and halftone are CSS
  gradients. There is not a single image file in the project.
- **Two audio tracks**, each on its own element and gain feeding one mixer, so
  moving between them is a real crossfade rather than a cut — with a master
  stage above both that still lets the mute button and the game-over duck apply
  to whichever is playing.
- The game's **landscape** is generated from a seeded PRNG, so the ink-wash
  scene is crisp at any resolution instead of being a fixed-size image.
- **Sound effects** are decoded into `AudioBuffer`s so hits can overlap during a
  combo, with the leading silence of each sample measured and trimmed — the bat
  sample had 89 ms of it, which read as input lag until it was skipped.
- Nothing plays until you press Start, because browsers require a gesture before
  any audio. The night screen is silent by design, so that press does double duty.

## Credits

Music generated with [Suno](https://suno.com). Bat and panda sounds from free
sound libraries.

The panda's line is a nod to the Egyptian *Panda Cheese* commercials, in which a
panda calmly destroys your kitchen when you decline the cheese.

A sibling of [mao-laskuri](https://github.com/oh6hfj/mao-laskuri), which opens by
pretending to be somebody else's homepage instead.

## A note on the Wall of Shame

The high-score table lists historical dictators with invented scores. The scores
are fiction; the reputations are not. It is satire, and the figures on it are
long dead.
