Hey Muse, I need you to finish and launch my VSL funnel's post-application routing on GitHub. Most of it is already built. Here's everything you need.

## Repo
- GitHub: https://github.com/hazzytrainer/hazzytrainer.southasianchad
- Work is on branch: `claude/github-account-setup-ud4run` (not merged into `main` yet)

## Files
| File | What it is |
|---|---|
| `index.html` | VSL landing page with the Tally form embedded (form ID `44eBGO`). Contains the routing script (search for `Tally.FormSubmitted`). |
| `thank-you-quality.html` | QUALIFIED applicants: confirmation, 4 "what happens next" steps, 8 client transformations (before/after photo + embedded YouTube video each), and a `DM Me "Applied"` button linking to `https://ig.me/m/hazzytrainer` |
| `application-received-unquality.html` | UNQUALIFIED applicants: thank-you message + 2 free Google Drive guides (Back 2" Wider, Arms 1" Wider) |
| `application-incomplete.html` | LOW-EFFORT answers: "We Need a Bit More" message + `Try Again` button back to `index.html#apply` |

## How the routing works
When the embedded Tally form is submitted, Tally sends a `Tally.FormSubmitted` postMessage to the page. The script in `index.html` reads the answers and redirects:

1. **`application-incomplete.html`** if BOTH of these answers are low-effort:
   - "What is your current goal in the next 12-24 weeks?…" (matched by `your current goal`)
   - "Currently what's been stopping you from achieving your goals?…" (matched by `stopping you from achieving`)
   - Low-effort means under 3 words or under 10 letters, keyboard mashing (asdf, qwer…), one letter repeated 4+ times, 6+ letter words with no vowels, or 3 or fewer unique letters. The Instagram handle question is never checked.
2. **`application-received-unquality.html`** if ANY of these is true:
   - Country = "India, Pakistan, Bangladesh" or "Other"
   - Age = "Less than 18"
3. **`thank-you-quality.html`** for everyone else.

Settings are at the top of the script: `MIN_WORDS`, `MIN_LETTERS`, `CHECKED_QUESTIONS`, `SKIPPED_QUESTIONS` and `DISQUALIFIERS`. The rules list only the answers that DON'T qualify, so renaming the qualifying options won't break anything.

## What's left for you to do
1. **Add the financial question as a third disqualifier.** Add an entry to `DISQUALIFIERS` with the question title and the answer options that should NOT qualify. I'll send you that question and which options fail.
2. **Verify the real Tally payload.** The routing logic was only tested with sample data, never against the live form. Temporarily add `console.log(msg.payload)` in the message listener, submit a test application, and confirm that:
   - `payload.fields[]` has `title`, `type` and `answer.value`
   - multiple-choice answers come through as text or as option IDs with an `options` list (`answerText()` handles both)
   - the question titles match the patterns above

   Remove the log when you're done.
3. **Merge** `claude/github-account-setup-ud4run` into `main` (open a PR and merge).
4. **Turn on GitHub Pages:** Settings → Pages → Deploy from branch → `main` / root. The repo must be public on a free plan. Optional: connect my custom domain.
5. **Test all 3 paths on the live URL** with real submissions:
   - Good answers + USA & Canada + 26-33 → `thank-you-quality.html`
   - Good answers + India, Pakistan, Bangladesh → `application-received-unquality.html`
   - Good answers + Less than 18 → `application-received-unquality.html`
   - Goal "abs" + blocker "idk" → `application-incomplete.html`
6. **Check on a phone:** the videos play inline, the `DM Me "Applied"` button opens an Instagram chat with @hazzytrainer, and both Google Drive guide links open for someone who is logged out (they must be shared as "Anyone with the link").

## Tally form fixes (in the Tally editor, not the code)
- Age options overlap: change "33-39" to **"34-39"**.
- Rename country option B to **"UK, Europe & Australia"** (Australia isn't in Europe).
- Optional: switch the goal and blocker questions to **Long answer** and set **Min characters ≈ 20**.
- Don't add a redirect inside Tally. The page code handles all redirects.

## Don'ts
- Don't rename the 3 disqualifying options ("India, Pakistan, Bangladesh", "Other", "Less than 18") or the goal/blocker question wording without also updating the patterns in the script.
- Don't add the routing logic in Tally. We decided the code on GitHub handles it.

Later (not now): add a "Book your call" calendar to `thank-you-quality.html`.
