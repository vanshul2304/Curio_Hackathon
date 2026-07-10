# OnRamp

**Your one next step into AI — not another catalog.**

OnRamp is a personalized "next step" guide for beginners breaking into AI. Instead of the
firehose of courses, newsletters, and event listings, it asks five quick questions (including
the one that matters: *what's really driving this?*) and hands back ONE short, ordered plan:
a single curated course to start tonight, plus 2–3 real upcoming events this month — each with
a beginner-safe tag, a "learn this first" link back to the course, and a one-line "why this
fits you" written from your own answers.

Why not just ask ChatGPT? ChatGPT gives you a plan and forgets you. OnRamp gives you the next
step, a real room to walk into this week, and a nudge when it's time. The bet: momentum comes
from one concrete step and one real room, not a 40-item listicle.

## Run it

Any static server from the repo root:

```
python3 -m http.server 8000
# open http://localhost:8000
```

No build step, no npm, no framework — vanilla HTML/CSS/JS by design.

## What's real vs. faked (honesty ledger)

- **Real:** the intake, the rules-based matcher (144-combo tested), curated course data with
  verified URLs, real event listings with real signup links, Save buttons (localStorage),
  shareable plan links (answers encoded in the URL hash).
- **Faked (fake-door):** "Email me my plan" only stores `{email, plan, answers}` to
  localStorage and shows a toast. No email is sent, no account exists, no weekly nudge loop
  is built. The UI copy never claims more than the toast line.

## File map

```
index.html              single page: landing → intake → plan (state machine, no router)
css/style.css           hand-rolled design system (dark + mint, one warm accent)
js/app.js               state machine, rendering, micro-interactions, inline fallback data
js/matcher.js           pure rules matcher: window.buildPlan(answers, courses, events)
data/courses.js         window.COURSES — curated courses (owned by data-courses lane)
data/events.js          window.EVENTS — real upcoming events (owned by data-events lane)
test/matcher-test.html  exhaustive matcher test: all 144 intake combos + city cases
```

If `data/courses.js` / `data/events.js` are missing or empty, `js/app.js` falls back to
4 inline sample courses + 4 sample events so the app always runs standalone.

## Matcher rules (short version)

Course: level match is a hard filter (never violated); then goal ×2, motivation ×2, hours ×1.
Events: beginner-safe strongly boosted for `level=new`, online-first unless your city matches,
sooner dates win ties, max 1 hackathon unless your goal is job/build. If filters starve the
pool, soft requirements are dropped in order (motivation → hours → goal for courses;
motivation → goal → beginner-preference → any upcoming online event for events) so the plan
is never empty. Open `test/matcher-test.html` to see every combo pass.
