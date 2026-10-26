# intent

**An Agent Skill that has Claude work out what you want before it starts.** Works with Claude Code, Claude, and other skill-aware assistants, for code and everyday requests alike.

The people who build Claude have converged on a short prompting rule: tell it what you want, how much effort to spend, and how it should check the result. Most requests carry one or two of those. `intent` runs the rule in reverse: it fills gaps from your code and context, and asks only where a wrong guess would cost real rework or reach other people.

## Install

```sh
npx skills add arjunlohan/intent        # project-scoped
npx skills add arjunlohan/intent -g     # global: available in every project
```

It triggers on its own at the start of long or expensive work, on vague or ambiguous requests, and when you ask Claude to interview you. You can also run `/intent`. Without installing, point any assistant at [`skills/intent/SKILL.md`](skills/intent/SKILL.md).

## What it does

1. **Builds a brief:** the goal, what done looks like and how it gets checked, the effort it deserves, and the limits, plus the assumptions it made.
2. **Reads the request for signals:** action or assessment verbs, quality words with no measure, effort words, audience words, and premises it can check.
3. **Scouts before asking.** The files, history, docs, and memory answer most questions.
4. **Sorts each gap by the cost of a wrong guess.** Cheap gaps get a stated assumption, costly ones a question, taste gets options to react to, and what nobody knows yet becomes a discovery step.
5. **Asks few, sharp questions** after doing the cheap work the answers can't change: up to four per round, highest stakes first, each with a default and a reason. After two rounds it shows a draft instead of asking more. An interview you ask for runs until the spec is done.
6. **Keeps the brief current** when a discovery breaks the premise or widens the scope, and **reports against it** at the end.

## What's inside

| File | Covers |
| --- | --- |
| [`SKILL.md`](skills/intent/SKILL.md) | The brief, the loop, when not to ask, and question craft |
| [`principles.md`](skills/intent/principles.md) | Twelve principles from how Claude's builders prompt it, each read from the receiving end, plus model-by-model differences |
| [`examples.md`](skills/intent/examples.md) | Eleven requests worked through the loop, from zero questions to a full interview |

## Where the principles come from

The sources are public posts, talks, and docs, checked against the originals in October 2026. This project is independent and is not endorsed by Anthropic or by the people cited.

| Principle | Sources |
| --- | --- |
| What, how hard, how to check | Boris Cherny, creator of Claude Code: [how he prompts, Oct 6 2026](https://x.com/bcherny/status/2107565388250874193); [goal, measure, effort, Oct 10 2026](https://x.com/bcherny/status/2109027327569203687) |
| A check the work can run | Boris Cherny: [setup thread, Jan 2 2026](https://x.com/bcherny/status/2007179832300581177). Anthropic: [Claude Code best practices](https://code.claude.com/docs/en/best-practices); [Demystifying evals for AI agents, Jan 2026](https://www.anthropic.com/research/demystifying-evals-for-ai-agents) |
| A coworker's brief, not a script | Boris Cherny: [Oct 6 2026](https://x.com/bcherny/status/2107565388250874193). Thariq Shihipar: [Lessons from Building Claude Code: How We Use Skills, Mar 2026](https://x.com/trq212/status/2033949937936085378). Anthropic: [Effective context engineering for AI agents, Sep 2025](https://www.anthropic.com/research/effective-context-engineering-for-ai-agents) |
| The reason behind the rule | Anthropic: [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices) |
| How hard to try | Boris Cherny: [an Opus 5.5 artifact prompt, Oct 6 2026](https://x.com/bcherny/status/2107532985897771152). Thariq Shihipar: [Spending your effort, Sep 2026](https://claude.dev/blog/spending-your-effort/) |
| Act or advise | Anthropic: [Prompting best practices](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices); [Prompting Claude Fable 5.1](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-1) |
| Check in only when readings diverge | Anthropic: [Prompting Claude Opus 5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5); [Prompting Claude Fable 5.1](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-1); Claude Code [permission modes](https://code.claude.com/docs/en/permission-modes) |
| Interview for big, fuzzy work | Thariq Shihipar: [spec interviews, Dec 28 2025](https://x.com/trq212/status/2005315275026260309); [A Field Guide to Fable: Finding Your Unknowns, Jul 2026](https://x.com/trq212/status/2073100352921215386); [Spending your effort, Sep 2026](https://claude.dev/blog/spending-your-effort/). Anthropic: [Claude Code best practices](https://code.claude.com/docs/en/best-practices) |
| Independent work first | Anthropic: [Prompting Claude Fable 5.1](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-1) |
| Show options for taste | Cat Wu and Boris Cherny of the Claude Code team on [Latent Space, May 2025](https://www.latent.space/p/claude-code) |
| Corrections compound | Boris Cherny: [tips from the Claude Code team, Jan 31 2026](https://x.com/bcherny/status/2017742741636321619). Anthropic: [Claude Code best practices](https://code.claude.com/docs/en/best-practices) |
| Trim as models improve | Thariq Shihipar: [The new rules of context engineering for Claude 5 generation models, Jul 2026](https://claude.dev/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models/) |
| Model differences | Anthropic's prompting guides for [Opus 5.5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5-5), [Fable 5.1](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-1), [Sonnet 5.5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-sonnet-5-5), and [Haiku 5.5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-haiku-5-5) |

Thariq Shihipar wrote these while on Anthropic's Claude Code team, which he left in October 2026.

## See also

- [**sharpen**](https://github.com/arjunlohan/sharpen) helps you write a prompt; `intent` is how Claude reads yours.
- [**finesse**](https://github.com/arjunlohan/finesse) is the design-engineering counterpart: motion, micro-interactions, and interface polish.
- [**claude.md**](https://github.com/arjunlohan/claude.md) is a working agreement whose first rule is a one-paragraph, always-on version of this loop. This skill holds the full procedure.

## License

[MIT](LICENSE) © Arjun Lohan
