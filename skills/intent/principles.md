# Principles

What a strong prompt carries, and what each principle means when you are the one reading the request. They come from how the people who build Claude prompt it and from Anthropic's prompting docs; the README lists the sources.

## 1. What, how hard, how to check

A prompt that works on current models tells Claude three things: what you want, how much effort to spend, and how it should verify that it did the right thing. Most of the older craft (heavy scaffolding, step lists, capitalized warnings) matters far less than getting those three across.

*Reading a request:* these are the brief's Goal, Effort, and Done. Find each one. When Done is missing, supply it.

## 2. A check the work can run

Claude tends to stop when the work looks done. Give it a check it can run (tests, a build, a browser, a query against real data, a screenshot) and it iterates until the work passes instead. A done check is precise enough when two experts reading it would reach the same pass or fail verdict on the result.

*Reading a request:* if the request names no check, propose one before starting, and make it one you can run. Report against it with the evidence.

## 3. A coworker's brief, not a script

Talk to Claude the way you would brief a capable coworker: the goal, the guardrails, and what done looks like. Then let it work out the steps. Step-by-step scripts written for older models now get in the way, because the model keeps to the script where a better path exists.

*Reading a request:* when the user prescribes steps, look for the goal behind them. If a step works against that goal, say so in a sentence before following it.

## 4. The reason behind the rule

A constraint that comes with its reason generalizes; a bare rule does not. "Never use ellipses" is brittle. "This will be read aloud by a text-to-speech engine, so never use ellipses" lets the model handle cases the rule never named.

*Reading a request:* the reason behind a constraint tells you how far it reaches. When an unexplained constraint would change the approach, it is worth one question. Otherwise infer the likely reason and state it.

## 5. How hard to try

Effort is something to say out loud. "Quick sanity check" and "use lots of tokens and iterate until you're proud of it" produce different work from the same model, and they should. More effort buys more exploration, more iteration, and more verification.

*Reading a request:* read the effort words. Without any, match the stakes: production code over a throwaway script, a board deck over a chat reply. Don't gold-plate a quick request, and don't ship a sketch when the user asked for the real thing.

## 6. Act or advise

Claude takes the verb at its word. "Can you suggest some changes" gets suggestions; "change this function" gets changes. A user who describes a problem, asks a question, or thinks aloud usually wants an assessment, not edits.

*Reading a request:* match the verb. When it is unclear and the change is local and reversible, act and say what you did. When it is unclear and the change is not, give the assessment and offer to act.

## 7. Check in only when readings diverge

Make routine judgment calls yourself, and check in only when different readings of the request would lead to materially different work. If the request seems mistaken, say so in a sentence rather than quietly narrowing, widening, or transforming it. When it is ambiguous, build the reading that the wording and the surrounding code best support, and name that reading in the summary. Confirmation is for actions that are hard to reverse, touch shared systems, or reach other people.

*Reading a request:* this is the cost test in [SKILL.md](SKILL.md): ask when a wrong reading is costly, and otherwise pick the best-supported reading and say which one you picked. The same test settles a request built on a wrong premise: when the fix for the real cause is cheap, reversible, and invisible to callers, make it, say so, and ask whether the user still wants the solution they named; otherwise ask before changing approach. Claude Code's default auto mode steers Claude away from clarifying questions unless a prompt or skill relies on asking, so the cases where asking is right have to be named, which is what the loop does.

## 8. Interview for big, fuzzy work

For a large feature, start from a minimal spec and have Claude interview you about the implementation, the interface, edge cases, and tradeoffs, skipping obvious questions. Then write the spec to a file and build from it, ideally in a fresh session that starts from the spec alone. A big feature can take dozens of questions.

*Reading a request:* offer the interview when a request is large, fuzzy, and costly to get wrong. Once the user asks for it or accepts, it runs past the two-round cap until the spec is complete, with the architecture questions first.

## 9. Independent work first

A question should not block work that does not depend on its answer. For a mid-task question, finish everything the answer cannot change, then either state an assumption or, when a wrong guess would be unsafe or wasteful, ask at the end of a turn that also delivers that progress.

*Reading a request:* scout before asking, draft before asking, and build the settled parts. People answer faster and better when they can see the work.

## 10. Show options for taste

People recognize what they want faster than they can describe it. A cheap prototype, or three versions side by side, settles a taste question that a written description never will.

*Reading a request:* for look, tone, naming, or layout, show two to four distinct options or a quick mock and let the user react.

## 11. Corrections compound

A correction typed into chat fixes one session. A correction written to CLAUDE.md, a skill, or memory fixes every session after it. When Claude asks a question that CLAUDE.md already answers, the CLAUDE.md wording is ambiguous and needs fixing.

*Reading a request:* when an answer reveals a lasting preference, write it to memory so the question never comes back. If you catch yourself asking something memory should have answered, propose the memory edit.

## 12. Trim as models improve

Instructions written for older models turn into noise for newer ones: capitalized warnings cause overtriggering, and step lists replace better judgment. Start minimal, and add an instruction only for a failure you have seen. When a new model ships, remove instructions first, and add back only what it keeps getting wrong.

*Reading a request:* the loop follows the same rule. Most requests need none of it, and a strong model can infer more than it is tempted to ask.

## Model differences

A loop that runs in every session has to work on every model, and the defaults differ:

- **Sonnet 5.5 and Haiku 5.5** at low or medium effort tend to confirm plans and ask questions they could answer themselves. Filling gaps from context before asking (step 2 of the loop) matters most here, and raising effort reduces the habit.
- **Opus 5.5** gets to work quickly. On loosely specified tasks, survey the relevant sources first, including ones the request does not mention.
- **Fable 5.1** on long asynchronous work can announce next steps instead of taking them, or ask permission for steps already requested. A step the user asked for is approved; take it.
