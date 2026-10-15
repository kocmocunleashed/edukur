# ED — follow your curiosity

A Next.js learning-feed design prototype with interactive experiments and independently themed lessons. Rage is the app's default; lesson designs include Dot, Terminal, Blueprint, Liquid Glass, Material You, and Y2K.

```sh
bun install
bun run dev --port 3100
```

Open http://localhost:3100. Browse the editorial feed, move the simulation sliders, open a lesson, answer its practice question, save it, or visit Create to swipe sideways through seven designs, then add your idea and source. Tap a lesson card to open its player: the experiment stays in view while you tap through short explanations and a quiz. Guest bookmarks stay on the device.

The current delivery focuses on the design and these interactions. Backend groundwork is included, but live model generation and the complete account/publishing workflow have not yet been tested end to end.

## Backend setup

Copy `.env.example` to `.env.local` and fill the server-only values. Without a database the public feed uses 12 local editorial lessons. With a database, run `bun run db:migrate`. This idempotently creates the tables and inserts editorial content. Do not point it at another application's database.

Local development has its own Neon project and development branch, separate from the school-report application. Managed Neon Auth uses email/password and email codes. Add the deployment origin to Neon trusted domains, set APP_URL, and configure production SMTP before deployment. Keep the cookie secret stable and at least 32 characters.

AI generation accepts any OpenAI-compatible endpoint, including DeepSeek. Add a model key to enable it. Generated JSON is schema-checked, repaired at most once, and reviewed before saving. Model review is fallible; publication remains an explicit author action. Original source text is private; selected source excerpts appear in the preview and published lesson. No model-generated code is executed.

The default caps are 5 generations per user per UTC day and 100 globally, including failed attempts. Sources support public HTML/text URLs, pasted text, and text PDFs up to 3 MB / 20 pages / 30,000 extracted characters. No OCR. The particle, circular orbit, and wave models are deliberately simplified and explained in the lessons.

## Feed ranking

See [the ranking design and attribution](docs/ranking.md). The feed uses an X-inspired retrieval/filter/score/diversity pipeline with transparent learning-oriented heuristic scores. It does not run X's trained Phoenix model. Latest offers chronological order.

## Checks

```sh
bun run typecheck
bun run lint
bun run build
# With the app running on port 3100 and Playwright Chromium installed:
node scripts/checks/design-flow.mjs
```

# edukur
