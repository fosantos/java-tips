# ☕ Java Tips — Cracking the Java Coding Interview

A static web page with **396 Java interview questions answered in plain words, with code** — derived from the
[Cracking the Java Coding Interview](https://www.youtube.com/playlist?list=PLzzeuFUy_Cngn3JZEXtu6G923y5v8y-8h)
shorts series by the [cours-en-ligne](https://www.youtube.com/@coursenlignejava) channel.

Every entry keeps the spoken format of the series:

- **Short answer** — the one-liner you can say out loud in an interview;
- **Less short answer** — the full explanation, including the presenter's "one last word" tips;
- the **code snippet** shown in the video (reconstructed from narration/frames);
- a **link to the original video**.

## Files

| File | Description |
|------|-------------|
| `index.html` | The whole site — self-contained (no dependencies, no build step): HTML + CSS + JS in a single file |
| `TUTORIAL.md` | Markdown source of all entries |

## Features

- Client-side **search** across all 396 questions (press `/` to focus, `Esc` to clear);
- **Jump to question** selector;
- dark mode (follows the system preference);
- every question links back to the original YouTube video.

## Publishing (GitHub Pages)

The page is a single static `index.html` — enable GitHub Pages once:

```bash
# repository Settings → Pages → Source: "Deploy from a branch" → main / (root)
```

or with the GitHub CLI:

```bash
gh api repos/fosantos/java-tips/pages -f "source[branch]=main" -f "source[path]=/"
```

The page will be served at <https://fosantos.github.io/java-tips/>.

## Numbering notes

The channel itself numbered two different questions with **96** and two with **399** (marked A/B here);
numbers **1, 264, 394, 396 and 397** have no video in the playlist; three videos are special episodes
with no question number. Questions appear in the order spoken in the videos ("Question number…").

## Credits & attribution

All questions, explanations and jokes come from the
[cours-en-ligne](https://www.youtube.com/@coursenlignejava) videos by José Paumard — full credit for the
content belongs to the author. The audios were automatically transcribed, then reviewed and edited into
this page; code snippets were reconstructed from the narration and video frames, so some identifiers may
differ from the originals.
