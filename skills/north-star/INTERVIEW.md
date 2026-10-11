# Interview

Run once, for a project with no vision doc. The doc it writes is what makes a second interview unnecessary.

1. Draft an answer to each question from the README, the code and the recent commits, so each question asks for a confirmation.
2. Ask everything in one message, with your drafts, then wait for the reply:
   - Who is this for, and what do they get?
   - What does success look like? Name two to five goals, each with a date or a number.
   - What is out of scope for now?
3. Push back once on an answer with no date or number ("soon", "make it good"). Take the second answer as given.
4. Write `docs/NORTH_STAR.md`, or `NORTH_STAR.md` at the root when there is no `docs/`:

   ```markdown
   # <Project>: north star

   <Who it's for and what they get, in two or three sentences.>

   ## Goals

   | Id | Goal | Done when | By |
   |---|---|---|---|
   | G1 | <outcome> | <checkable signal> | <YYYY-MM-DD> |

   ## Out of scope

   - <item>
   ```

5. Add this line to `CLAUDE.md`, or to `AGENTS.md` when the project uses that instead. Create `CLAUDE.md` when neither exists.

   ```
   Before planning work, read `<path to the doc>`: who the product is for and the goals it is judged on.
   ```

6. Leave both files uncommitted and say so in one line above the screen.

Done when the doc has at least one goal with a date or a number and the pointer line is in place.
