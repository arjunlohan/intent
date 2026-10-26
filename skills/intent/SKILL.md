---
name: intent
description: Works out what the user wants before non-trivial work starts. Reads each request for its goal, what done looks like and how to check it, how much effort it deserves, and its limits; fills gaps from context first, states cheap assumptions and proceeds, shows options for matters of taste, and asks only what the user alone can answer. Use at the start of a long, expensive, or hard-to-reverse task; when a request is vague ("make it better", "clean this up", "fix the UX") or open to two readings; when the user asks to be interviewed or says "ask me questions", "make sure you understand", or "clarify before you start"; and mid-task when a discovery changes the scope or breaks the request's premise. Skip it for clear, small, or easily undone requests. To write or improve a prompt, use sharpen instead.
---

# Intent

A strong prompt says what to do, how much effort it deserves, how the result will be checked, and what its limits are. Most requests arrive with one or two of those. This skill reads each non-trivial request for all four and closes every gap the cheapest safe way: from context, with a stated assumption, by showing options, or with a question when nothing else will do.

What you are managing is the cost of a wrong guess. A question costs the user a round trip. A wrong guess costs rework, and sometimes something nobody can take back. Spend questions where a wrong guess is expensive, and nowhere else.

## The brief

Before non-trivial work starts, hold those four, plus the assumptions you make to fill them:

| Field | Answers | Usually found in |
| --- | --- | --- |
| **Goal** | What outcome, for whom, and why it matters | The request, the conversation so far, the issue or doc it links |
| **Done** | What finished looks like, and how it gets checked | Tests, a metric, a screenshot, a check a person can run; propose one when none is given |
| **Effort** | How much time, compute, and polish the work deserves | Words like "quick" or "make it great"; otherwise the stakes |
| **Limits** | What to leave alone, and what needs a yes first | Scope words in the request, CLAUDE.md or AGENTS.md, anything irreversible or seen by other people |
| **Assumptions** | What you decided without asking | You, stated where the user will see it |

Done is the field most often missing and the most valuable to supply. A check the work can run against turns "looks right" into "is right", and it keeps every later progress report honest.

## The loop

1. **Read the request for the brief.** Take each field from the words first, then from what the words imply.
   - *Verbs.* "Fix", "add", and "change" ask for action. "Suggest", "review", and "what do you think" ask for an assessment. Do the one that was asked; don't quietly turn a review into edits, or an edit into a list of suggestions. When the verb is unclear, the cost test in step 3 decides.
   - *Quality words with no measure* ("better", "cleaner", "faster", "production-ready"): Done is missing.
   - *Effort words:* "quick", "rough", and "just" pull one way; "really good", "iterate until it's right", and "don't stop until" pull the other.
   - *Audience words:* "for the board", "post this", "send it to customers". The work leaves the room, so the Limits tighten.
   - *A checkable premise* ("the bug is in the parser", "it's slow because of X"): check it before building on it. If it is wrong, say so first, then sort the fix for the real cause with the cost test in step 3.
2. **Fill gaps from context before asking anything.** Read the files the request touches, the nearest similar code or document, the git log, AGENTS.md, memory, earlier messages, and attachments. Search the web for public facts. A question the repo could answer costs the user time, and it costs you their trust in every later question.
3. **Sort what is still missing by the cost of a wrong guess.**

   | The gap | The move |
   | --- | --- |
   | Cheap to undo: local, reversible, minutes of rework | Assume. State the assumption in one line and proceed. |
   | Costly: hours of rework, a data model or interface, money, other people's systems, anything public or irreversible | Ask. |
   | Taste: the user will know it when they see it | Show two to four distinct options, a quick mock, or a reference to react to. Don't ask them to describe it. |
   | Unknown to the user as well | Make it a discovery step: check it first, then branch on what you find. |

4. **Ask few, sharp questions.** First do the cheap parts the answers can't change, so the questions arrive with progress. Then ask up to four per round, one decision each (compound questions get half-answers), highest stakes first: data and interfaces before behavior, behavior before wording. Give every question your default and a one-clause reason, so accepting all of them is one action. Use the harness's question tool when there is one, with the recommended option first; without one, ask in plain text at the end of the turn. Two rounds at most; then stop asking and show something concrete: a written brief, a draft, or a prototype. These caps bound the questions you volunteer. In an interview the user asked for or accepted, put the architecture questions first, keep going until the spec is complete, and write the spec to a file.
5. **State the brief, then go.** For a small task, one line of assumptions. For long or expensive work, put the brief where the user will see it before the work gets far: the plan, the task list, or the first message.
6. **Keep the brief current.** Re-open it when a discovery breaks the premise, the right fix needs wider scope, the done check proves wrong or impossible, or the user corrects course mid-task. For a small correction, adjust and continue. For a scope change or a broken premise, say so and ask before widening.
7. **Report against the brief.** Close on the done check: what passed and how you know, what you could not verify, and which assumptions the user should look at.
8. **Record what you learn about the user.** When an answer reveals a lasting preference rather than a one-off choice, save it where the user's instructions route lessons (the harness's memory by default, asking first if they say to), so the question never needs asking twice.

## When not to ask

- The request is clear, or every wrong guess on it is cheap to undo.
- It is a bug report with logs or a failing check. The intent is "make it pass without breaking anything else"; start from the evidence.
- The user already answered, or said "your call" or "just do it". Decide, and state the assumption.
- It asks permission for a local, reversible step. Take the step and say what you did.

Never end a turn on a question you could have answered yourself.

## Question craft

A good clarifying question names one decision, offers a default, and gives the reason:

- "The export can stream rows (fast, no totals row) or build the file in memory (has the totals row, fails above about 200k rows). Your largest table has 1.2M rows, so I'll stream unless you need the totals row."
- "Should this go to #general (1,200 people) or #eng first? Default: #eng, because a post to #general can't be taken back."

Never hand the brief back with "What are the requirements?": draft it yourself and ask only about its gaps.

## Beyond code

The loop is the same for a document, an analysis, a plan, or an email. Done becomes a check a person can run: "fits on one slide", "a new hire can act on it without a follow-up question", "three options with their costs". Context is the attachment, the data, the previous draft. A blank chat has little to read, so questions carry more of the load there; the cost test still decides whether to ask one.

## Reference files

| File | Read it when |
| --- | --- |
| [principles.md](principles.md) | The reason behind a step would settle an edge case, or you need the model-by-model differences |
| [examples.md](examples.md) | You can't tell which move a request gets, or it is a document, message, or design rather than code |
