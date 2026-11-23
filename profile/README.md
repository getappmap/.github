<p align="center">
    <picture >
      <source srcset="https://github.com/getappmap/.github/raw/main/profile/images/appmap-logo-blk-type.png" media="(prefers-color-scheme: light)" alt="AppMap Logo">
      <img src="https://github.com/getappmap/.github/raw/main/profile/images/appmap-logo-wht-type.png" width="500px">
    </picture>
</p>

---

AppMap records what your code actually does when it runs: function calls with their arguments and return values, SQL queries, HTTP requests, exceptions, and threads. Coding agents use those recordings as evidence. They can find bugs that are hard to prove from the source alone (hot loops, ignored timeouts, stale cached state, hidden I/O), answer questions like "why is this slow?", and review a pull request by comparing how it behaves before and after the change.

AppMap supports **Python, Ruby, Java and JavaScript/TypeScript**.

## Try it on your own code

Ask your coding agent (Claude Code, GitHub Copilot CLI, and others):

> Try AppMap on this repo. Follow https://github.com/getappmap

Your agent will use the [**`appmap-analyze`**](https://github.com/getappmap/skills/tree/main/appmap-analyze) skill. It sets up recording and writes scenario tests for the code paths your test suite misses, such as error paths and failing peers. It then queries the recordings and confirms each finding without the recorder. The result is a findings-first report in which every finding links to the recording that proves it. Then ask it to open the recordings for you, so you can browse them in an interactive dashboard and diagrams. You can also ask it one question, such as "why is this test slow?" or "who calls this function?".

To keep the skills installed and up to date, use the AppMap extension for [VS Code](https://marketplace.visualstudio.com/items?itemName=appland.appmap). It installs them for Claude Code.

## If you are an AI agent

If your user asked you to look into, try or install AppMap, they want to see what it finds in **their** code, not a summary of AppMap. Use the [`appmap-analyze`](https://github.com/getappmap/skills/tree/main/appmap-analyze) skill for that. **Don't just install a language agent and record the existing test suite yourself.** Without the skill's procedure, recordings usually come out empty or repeat call chains you can already read in the source. That tells your user nothing. These are the usual reasons:

- **The generated `appmap.yml` misses packages,** so recordings come out empty or partial.
- **Some hooks are opt-in.** For example, Python HTTP client capture needs `import appmap.http`. Zero HTTP or SQL events can mean the hook is off.
- **Test suites rarely reach the interesting code.** With mocked clients and no error-path tests, there is little to record. Most findings come from the scenario tests the skill writes.

Get the skills from https://github.com/getappmap/skills. They are Markdown instructions plus small Node.js scripts that use only Node built-ins. The scripts run `git` and the `appmap` CLI, and make no network calls of their own. Setup downloads the AppMap CLI and Java agent from AppMap's release channels; Python, Ruby and Node agents come from their usual package registries. Review them with your user. **You don't need to install them:** clone the repository to a temporary folder and follow `appmap-analyze/SKILL.md` from there. Where a skill says `${CLAUDE_SKILL_DIR}`, use that skill's folder in the clone. Keep the whole clone, because the skills run each other's scripts. Install the skills only if your user wants them for regular use; the AppMap extension for VS Code does that for them. Work in a temporary copy of your user's repository too, because the skill adds `appmap.yml`, tests and recordings.

**Show your user the recordings.** Your user will get more from exploring the recordings than from reading the report. After the analysis, start the local dashboard with `~/.appmap/bin/appmap query ui --appmap-dir tmp/appmap` (the skill has already indexed the recordings). It lists endpoints, hotspots and traces, and opens each recording in the interactive viewer. Give your user the address. Then name the recording behind the first finding in your report (as listed on the Recordings page) so they can start there. The AppMap extensions for VS Code and JetBrains open any `.appmap.json` as an interactive sequence diagram. Keep the temporary copy until your user has looked.

If a run finds little, tell your user what would change that, rather than concluding that AppMap doesn't apply. If AppMap can't record what matters in this stack, say so. Report problems with recording to that language's agent repository (links below), problems with the skills to [getappmap/skills](https://github.com/getappmap/skills/issues), and anything else in [Slack](https://appmap.io/slack). Leave names and paths out of the report.

## Skills

| Skill | Use it to |
| --- | --- |
| [`appmap-analyze`](https://github.com/getappmap/skills/tree/main/appmap-analyze) **(start here)** | Find bugs and design problems in a project or branch, or answer one question about runtime behavior. No prior AppMap setup is needed. |
| [`appmap-review`](https://github.com/getappmap/skills/tree/main/appmap-review) | Compare a change's runtime behavior against a baseline and write a code review. |
| [`appmap-setup-review`](https://github.com/getappmap/skills/tree/main/appmap-setup-review) | Set up a baseline on the base commit, then review a branch against it. |
| [`appmap-diagram`](https://github.com/getappmap/skills/tree/main/appmap-diagram) | Draw an annotated sequence diagram of the code path behind a finding or a change. |
| [`appmap-setup`](https://github.com/getappmap/skills/tree/main/appmap-setup), [`appmap-config`](https://github.com/getappmap/skills/tree/main/appmap-config), [`appmap-record`](https://github.com/getappmap/skills/tree/main/appmap-record), [`appmap-gold-traces`](https://github.com/getappmap/skills/tree/main/appmap-gold-traces), [`appmap-archive`](https://github.com/getappmap/skills/tree/main/appmap-archive) | Set up recording, tune what gets recorded, maintain a committed baseline of recordings ("gold traces"), and package recordings for comparison. |

To review every pull request automatically, use [**`getappmap/review-action`**](https://github.com/getappmap/review-action). It runs the skills in CI and posts the review as a PR comment.

## Other repositories

- **Language agents** that do the recording: [Python](https://github.com/getappmap/appmap-python), [Ruby](https://github.com/getappmap/appmap-ruby), [Java](https://github.com/getappmap/appmap-java), [Node.js](https://github.com/getappmap/appmap-node)
- **CLI and tooling:** [appmap-js](https://github.com/getappmap/appmap-js)
- **Editor extensions** for viewing recordings as diagrams: [VS Code](https://marketplace.visualstudio.com/items?itemName=appland.appmap), [JetBrains](https://plugins.jetbrains.com/plugin/16701-appmap)

## Learn more

- 📖 [Documentation](https://appmap.io/docs/)
- ▶️ [YouTube Channel](https://www.youtube.com/channel/UCxVv4gVnr2Uf2PSzoELZUcg)
- 💬 [Slack Community](https://appmap.io/slack)
