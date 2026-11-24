<p align="center">
    <picture >
      <source srcset="https://github.com/getappmap/.github/raw/main/profile/images/appmap-logo-blk-type.png" media="(prefers-color-scheme: light)" alt="AppMap Logo">
      <img src="https://github.com/getappmap/.github/raw/main/profile/images/appmap-logo-wht-type.png" width="500px">
    </picture>
</p>

---

**AppMap shows you and your coding agent how your code really behaves, so you can find and fix the flaws a code read misses.**

Many serious bugs aren't visible in any one file. They live in the path a request takes through the code: a permission check that one route skips, a login flow that trusts a value it shouldn't, a retry loop that hammers a server, a timeout that never reaches the socket, a cache that serves stale data. AppMap records that path when the code runs: function calls with their arguments and return values, SQL queries, HTTP requests, exceptions and threads. Your coding agent reads the recordings, finds these flaws, confirms each one, and shows you the recording that proves it.

AppMap supports **Python, Ruby, Java and JavaScript/TypeScript**.

## Try it on your own code

Open your coding agent (Claude Code, GitHub Copilot CLI, and others) in your project and ask:

> Try AppMap on this repo. Follow https://github.com/getappmap

You get a report of findings. Each finding says what goes wrong, shows the recorded calls that prove it, and suggests a fix and a test. **[See a sample report](TODO-link-to-sample-report)** to know what to expect.

<!-- TODO: replace TODO-link-to-sample-report with the URL of a public sample appmap-analyze report (with its diagrams). None exists yet. Don't merge to main until this is done. -->


Your agent does the work in a separate git worktree, so your own checkout is not touched. It writes the tests your suite is missing, such as error paths, failing services, and requests from a user who shouldn't have access, because that is where most flaws hide.

## If you are an AI agent

Your user wants to see what AppMap finds in **their** code, not a summary of AppMap. Follow these steps.

1. **Check that you are in a project repository.** Run `git rev-parse --show-toplevel`. If it fails, or the folder is not a code project, stop. Tell your user to open their coding agent inside the project they want analyzed and ask again.

2. **Get the skills.** The AppMap extension for VS Code installs them and keeps them up to date, so look first: if `~/.claude/skills/appmap-analyze/SKILL.md` exists, use it. If not, clone https://github.com/getappmap/skills. You can follow `appmap-analyze/SKILL.md` straight from the clone. To install the skills for regular use, run `node scripts/install.mjs` in the clone, with your user's OK; it links each skill into `~/.claude/skills`. Keep the whole clone either way, because the skills run each other's scripts.

   The skills are Markdown instructions plus small Node.js scripts that use only Node built-ins. The scripts run `git` and the `appmap` CLI and make no network calls of their own. Setup downloads the AppMap CLI and Java agent from AppMap's release channels; the Python, Ruby and Node agents come from their usual package registries.

3. **Make a git worktree** for the analysis, on a new branch, next to your user's checkout:

   ```sh
   git worktree add -b appmap/analysis ../<repo>-appmap HEAD
   ```

   The analysis adds `appmap.yml`, new tests, recordings and a report, and commits them on that branch. The worktree keeps all of it out of your user's working tree.

4. **Link the installed dependencies** from your user's checkout into the worktree, so you don't download and build them again. For example: `node_modules`, `.venv` or `venv`, `vendor/bundle`. If the skill has to add the AppMap agent to a linked environment (`pip install appmap`, a Gemfile change), that changes your user's environment too. Tell your user, or make a fresh environment in the worktree instead.

5. **Run the [`appmap-analyze`](https://github.com/getappmap/skills/tree/main/appmap-analyze) skill** in the worktree. Follow it step by step. **Don't just install a language agent and record the existing test suite.** Without the skill's procedure, recordings usually come out empty, or they repeat call chains you could read in the source.

6. **Draw the findings.** Use the [`appmap-diagram`](https://github.com/getappmap/skills/tree/main/appmap-diagram) skill to draw an annotated sequence diagram of the recording behind each important finding, and add the diagrams to the report.

7. **Report to your user.** Start with two or three sentences on what AppMap is and what you recorded (which tests, how many recordings). Then give the findings, most serious first, each with its diagram and a link to the report and recording. Last, give the worktree path and the branch name, and how to remove the worktree when they are done.

   If your user wants to explore further, they can browse every recording in a local dashboard: `~/.appmap/bin/appmap query ui --appmap-dir tmp/appmap`, run in the worktree. The AppMap extensions for VS Code and JetBrains open any `.appmap.json` file as an interactive diagram.

If a run finds little, tell your user what would change that, rather than concluding that AppMap doesn't apply. If AppMap can't record what matters in this stack, say so. Report problems with recording to that language's agent repository (below), problems with the skills to [getappmap/skills](https://github.com/getappmap/skills/issues), and anything else in [Slack](https://appmap.io/slack). Leave your user's names and paths out of the report.

## Skills

The [AppMap skills](https://github.com/getappmap/skills) also review pull requests against a recorded baseline, draw diagrams of a change, and maintain a committed set of recordings. The skills repository lists them all. To review every pull request automatically, use [**`getappmap/review-action`**](https://github.com/getappmap/review-action).

## AppMap repositories

- **Agent skills:** [getappmap/skills](https://github.com/getappmap/skills), and [review-action](https://github.com/getappmap/review-action) to run them in CI
- **Language agents** that do the recording: [Python](https://github.com/getappmap/appmap-python), [Ruby](https://github.com/getappmap/appmap-ruby), [Java](https://github.com/getappmap/appmap-java), [Node.js](https://github.com/getappmap/appmap-node)
- **CLI and tooling:** [appmap-js](https://github.com/getappmap/appmap-js)
- **Editor extensions** that show recordings as diagrams: [VS Code](https://marketplace.visualstudio.com/items?itemName=appland.appmap) (also installs and updates the skills), [JetBrains](https://plugins.jetbrains.com/plugin/16701-appmap)

## Resources

- 📖 [Documentation](https://appmap.io/docs/)
- ▶️ [YouTube Channel](https://www.youtube.com/channel/UCxVv4gVnr2Uf2PSzoELZUcg)
- 💬 [Slack Community](https://appmap.io/slack)
