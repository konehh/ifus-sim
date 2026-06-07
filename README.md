# Sim — turn your notice period into a portfolio

Sim is a job simulator for software engineers. You drop in a resume, get a ladder of
real-world, company-style project briefs calibrated to your own stack — graded from
warm-up to brutal — and every project you finish lands on a single shareable portfolio
page you can send to employers.

The core idea is the **artifact gate**: a project only counts as done when there's
something a stranger can open (a repo, a live demo, a write-up). The portfolio only ever
shows real, openable work.

**Live site:** _add your GitHub Pages URL here, e.g._ `https://yourusername.github.io/sim`

---

## What's in this repo

This is a static site — plain HTML, CSS, and JavaScript, no build step, no framework.
That keeps it free to host and easy to edit.

| File | What it is |
|------|------------|
| `index.html` | Landing page. The pitch, the resume drop, and the three-step "how it works". |
| `ladder.html` | The project ladder. Shows all projects as cards, 10 per page, sortable by latest or difficulty, each expanding to a full brief. |
| `portfolio.html` | The shareable portfolio page. Starts empty; fills with cards as projects are shipped. The one URL you send to employers. |
| `ship.html` | The "ship a project" form. Where a finished project's links are pasted to add it to the portfolio. |
| `projects.js` | The project data — all the briefs live here as one list. **This is the only file you edit to add or change projects.** |

---

## How it works (the flow)

1. **Landing** (`index.html`) — explains Sim and points to the ladder.
2. **Ladder** (`ladder.html`) — reads the briefs from `projects.js` and displays them.
   Cards are colour-coded by difficulty (green warm-up, blue core, amber hard, coral
   brutal) and by archetype (the pill colour).
3. **Ship** (`ship.html`) — a form with title, outcome, archetype, sector, stack, a
   required repo link, and an optional live demo link. The "Add to portfolio" button is
   disabled until a title and repo link are present — that's the artifact gate.
4. **Portfolio** (`portfolio.html`) — on submit, the new project is added as the top card
   and the header stats update.

---

## Adding or changing projects

All briefs live in `projects.js` as a list of objects. To add projects, edit that one file
— nothing else needs to change. Each project looks like this:

```js
{
  n: 31,                                  // unique number (higher = newer)
  tier: "Core",                           // Warm-up | Core | Hard | Brutal  (drives colour)
  archetypeKey: "build",                  // build|polish|debug|rescue|ai|architect|unfixable|capstone (pill colour)
  archetype: "Build-type · integration",  // label shown on the pill
  sector: "Retail",
  title: "Project title",
  summary: "One-line summary shown on the card.",
  stack: ["C#", ".NET 8", "EF Core"],     // tech badges
  brief: "The full scenario, written like a real company brief.",
  builds: ["What he builds, point 1", "point 2"],
  effort: "S — about 2–3 person-days. ...",
  cost: "Near-zero infra. One developer. ...",
  accept: ["Acceptance criterion 1", "criterion 2"],
  questions: ["A question a delivery manager would ask", "another"],
  stretch: "An optional harder extension."
}
```

The ladder paginates automatically (10 per page), so adding more projects just adds more
pages — no other change needed.

---

## Running it locally

Because it's a static site, you can open `index.html` directly in a browser to preview.
For the page-to-page links to behave exactly as they do when deployed, it's best to serve
the folder over a tiny local web server. With Python installed:

```bash
# from inside the project folder
python -m http.server 8000
# then open http://localhost:8000 in your browser
```

---

## Deploying (GitHub Pages)

1. Put all the files at the **root** of the repo (not inside a subfolder).
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to "Deploy from a branch".
4. Set **Branch** to `main` and folder to `/ (root)`, then **Save**.
5. Wait ~1 minute, then visit `https://yourusername.github.io/<repo-name>`.

To update the live site, edit a file and commit the change — GitHub Pages rebuilds
automatically within a minute. If you see an old version, do a hard refresh
(`Ctrl+Shift+R`, or `Cmd+Shift+R` on Mac) to bypass the browser cache.

---

## Known limitations (and what fixes them)

This is the no-code prototype. It is deliberately front-end only:

- **Shipped projects don't persist.** They live in the browser session, so the portfolio
  resets when the session ends. There is no database yet.
- **The portfolio isn't truly shareable cold.** Projects shipped on one machine won't show
  on another, because there's no server storing them.
- **The resume upload is a placeholder.** It doesn't yet read a resume and generate
  projects automatically.

All three are solved the same way: a real backend with a database and server-side logic.
That is the capstone project on the ladder — *"Rebuild Sim for real"* — which adds resume
parsing, project generation, persistence, authentication, and the artifact gate enforced
in server code rather than in the browser.

---

## Stack

- HTML, CSS, vanilla JavaScript
- Fonts: Sora (headings) + Inter (body), via Google Fonts
- Hosted on GitHub Pages
- No build step, no dependencies

---

_Sim started as a no-code prototype and is designed to be rebuilt into a real product as a
portfolio capstone._
