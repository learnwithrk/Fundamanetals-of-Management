# Fundamentals of Management — Quiz Hub

A static, GitHub Pages-friendly quiz site. Name-based login, per-topic MCQ
quizzes, a shared leaderboard, and a full review of wrong answers at the
end of every attempt. Add a new topic any time by dropping in one JSON
file — no code changes needed.

## What's included

```
index.html          Home page — lists every quiz from quizzes/manifest.json
quiz.html            Quiz runner (?id=<quiz-id>)
leaderboard.html      Standalone leaderboard view (?id=<quiz-id>)
css/style.css         Shared styling
js/app.js             Login, header, leaderboard read/write
js/quiz-engine.js     Quiz rendering, scoring, review screen
js/firebase-config.js Your Firebase keys go here (see below)
quizzes/manifest.json List of published quizzes
quizzes/5-1-introduction-to-management.json  First quiz (44 questions)
firestore.rules.txt   Security rules to paste into Firebase
```

## 1. Publish it on GitHub Pages

1. Copy every file above into your repo
   `learnwithrk/Fundamanetals-of-Management` (keep the folder structure).
2. In the repo: **Settings → Pages → Source → Deploy from a branch**,
   branch `main`, folder `/ (root)`. Save.
3. Your site will be live at:
   `https://learnwithrk.github.io/Fundamanetals-of-Management/`

The site works immediately even before you do step 2 below — it just
falls back to a **local-only leaderboard** (saved in each visitor's own
browser) and shows a banner saying so.

## 2. Turn on the shared leaderboard (Firebase, free)

Takes about 5 minutes, no credit card:

1. Go to <https://console.firebase.google.com> → **Add project** → give
   it any name → finish the wizard.
2. Left menu → **Build → Firestore Database** → **Create database** →
   choose **Start in production mode** → pick any region → **Enable**.
3. In Firestore, open the **Rules** tab, delete the default rules, and
   paste in everything from `firestore.rules.txt` in this repo → **Publish**.
4. Back on the **Project Overview** page, click the `</>` (web) icon to
   register a web app. You don't need Firebase Hosting — just copy the
   `firebaseConfig` object shown.
5. Open `js/firebase-config.js` in your repo and replace the placeholder
   values with the real ones from step 4. Commit.

That's it — reload the site and the leaderboard is now shared across
every visitor. The config values are meant to be public in a
client-side app like this one; the Firestore **rules** are what keeps
the data safe (read-only for everyone except well-formed new scores).

## 3. Add a new topic quiz later

1. Duplicate `quizzes/5-1-introduction-to-management.json` and rename it,
   e.g. `quizzes/5-2-functions-of-management.json`.
2. Edit its `id`, `title`, `subtitle`, and `questions` array. Each
   question looks like:
   ```json
   {
     "q": "Question text?",
     "options": ["Option A", "Option B", "Option C", "Option D"],
     "answer": 0
   }
   ```
   `answer` is the zero-based index of the correct option (0 = first
   option). Every quiz on this site uses exactly 4 options and never
   includes "None of the above".
3. Add one entry to `quizzes/manifest.json`:
   ```json
   {
     "id": "5-2-functions-of-management",
     "title": "5.2 Functions of Management",
     "subject": "Fundamentals of Management",
     "file": "quizzes/5-2-functions-of-management.json",
     "questionCount": 30
   }
   ```
4. Commit and push. The new quiz appears on the home page automatically
   — no other file needs to change.

## How scoring & review work

- Every question is single-select, 4 options, one correct answer.
- On submit, unanswered questions count as incorrect (with a warning
  before submitting).
- The results screen shows the total score, then — for every question
  answered incorrectly — the question text, the learner's answer, and
  the correct answer.
- The attempt is then saved (name, quiz, score, timestamp) and the
  top 10 scores for that quiz are shown right below the review.

## Notes on the first quiz

`5-1-introduction-to-management.json` covers **Section 5.1 "Introduction
to Management"** — definitions of management by eight authors, the 15
features/nature of management, management as a science vs. an art,
scope of management, and management as an emerging profession. 44 MCQs
total.
