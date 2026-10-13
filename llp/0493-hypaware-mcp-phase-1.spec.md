# LLP 0493: HypAware MCP, Phase 1 (client)

**Type:** Spec
**Status:** Draft
**Systems:** MCP, CLI, Plugins, Docs
**Author:** HypForge designer
**Date:** 2026-10-13
**Source:** [HYP-128 "HypAware MCP: close the agent access gap"](https://linear.app/hyperparam/issue/HYP-128/hypaware-mcp-close-the-agent-access-gap),
Phase 1, description at revision 2026-10-13T18:49:06.490Z; client sub-issues
HYP-130 and HYP-132 to HYP-139
**Related:** LLP 0494 (design), LLP 0034 (MCP hosting), LLP 0298 (the CI
mint), LLP 0107 (skills ride attach), LLP 0197 (skills state constraints),
LLP 0491 (team history guidance); hypaware-server LLP 0595 to 0598 (the
server half, out of tree); hypscope LLP 0104 (the cloud-agent recipe, out of
tree)

> Draft 2026-10-13. Decision owner: Hernan Alvarez, through the mayor.
> Phase 1 is approved; Phases 2 and 3 are held until a recorded sync with
> Hernan. HYP-140 (the hypaware.ai pages) is held until Phil grants the
> hyparam/hypaware.ai route. HYP-135 is designed without HYP-75, which is
> not in this mission. This document covers the client half of Phase 1. The
> parent issue text and the two server sub-issues (HYP-129, HYP-131) are
> quoted in hypaware-server LLP 0595#request.

## Request {#request}

Each sub-issue body below is quoted from Linear in full, at the revision
above. The quotes are reflowed to fit; the words are Linear's.

**HYP-130, "MCP 1.2: `hyp remote mint --scope query` and read token in the
cloud-agent recipe":**

> Phase 1, hypaware client and HypScope.
>
> Expose the personal read token (MCP 1.1) from the CLI as
> `hyp remote mint --scope query`, and include it in the HypScope "Connect a
> cloud agent" setup recipe from HYP-116, so a cloud agent can read team
> history and recommendations, not only be recorded.
>
> Done when: the CLI mints a read token, and the HYP-116 recipe places it in
> the environment's secret store alongside the enrollment token.

**HYP-132, "MCP 1.4: MCP server instructions block":**

> Phase 1, hypaware client (src/core/mcp/server.js, which the server
> reuses).
>
> Our MCP server sends no `instructions` today; it answers initialize, tools
> and resources only. Add an instructions block that tells any agent when to
> use the HypAware tools and the summary-first rule, so clients without our
> skill still know how to use them.
>
> Done when: initialize returns instructions on both stdio and /v1/mcp.

**HYP-133, "MCP 1.5: Agent commands as MCP prompts":**

> Phase 1, hypaware client (src/core/mcp/server.js, prompts capability,
> which the server reuses).
>
> Add MCP prompts that appear as commands in the agent (slash commands in
> Claude Code), each the counterpart of a CLI command:
>
> * fix a recommendation by id (`hyp report fix`)
> * list open recommendations for this repo (`hyp report list`)
> * check team history before starting a task (`hyp ask`)
>
> The server has no prompts/list or prompts/get today.
>
> Done when: the three commands appear in Claude Code and Cursor connected
> over stdio and over /v1/mcp.

**HYP-134, "MCP 1.6: Ship existing skills to Pi, OpenCode, Cursor and
OpenClaw":**

> Phase 1, hypaware client.
>
> Skills (hypaware-query, -reference, -privacy, -report) target only
> `claude` and `codex` in their plugin manifests. Pi, OpenCode, Cursor and
> OpenClaw already declare a skill_dir, so the installer can place skills
> there; nothing lists them.
>
> Change: add those clients to each skill's client list, and check the
> skill content works in each (LLP 0491 retrieval method names both CLI and
> MCP surfaces).
>
> Done when: `hyp client skills install` places the skills for each attached
> client and an agent in each uses them correctly.

**HYP-135, "MCP 1.7: Skill or knowledge entry for Devin and Factory
Droid":**

> Phase 1, with the HYP-75 software-factory recipes.
>
> Devin and Droid can run the CLI and Devin takes a remote MCP org-wide
> (bearer header or OAuth), but neither gets our skill. Write the equivalent
> instructions in each vendor's format (Devin knowledge, Droid skill) so the
> agent knows when to read team history and act on recommendations.
>
> Done when: the HYP-75 pilot recipes include the instructions and the read
> token.

**HYP-136, "MCP 1.8: Docs agent prompts work through the MCP, plus a prompt
for `hyp report list`":**

> Phase 1 docs, hypaware/docs (Reports, Querying, Team setup). Copy agent.
>
> Docs release hyparam/hypaware#2570 added a CLI/agent switch on 22 command
> blocks with a copyable "Prompt for your agent." Those prompts tell the
> agent to run `hyp`, so they assume a shell and the CLI. Extend each so it
> works through the HypAware MCP tools when the agent has no `hyp`. Add the
> missing agent prompt for `hyp report list`.
>
> Done when: every agent prompt names the MCP path, and `hyp report list`
> has a prompt.

**HYP-137, "MCP 1.9: Full docs pass, add the MCP path beside every CLI
instruction":**

> Phase 1 docs, hypaware/docs. Copy agent.
>
> Wherever a page says to run `hyp` for reading history or acting on
> recommendations, add the MCP path beside it: Querying, Reports, Team
> setup, Clients, Troubleshooting. Today MCP is covered by four stdio-only
> lines in Querying and the generated /docs/cli-reference/serve-mcp-tools
> page.
>
> Done when: no read or act instruction in the docs is CLI-only.

**HYP-138, "MCP 1.10: New page /docs/mcp":**

> Phase 1 docs. New file hypaware/docs/MCP.md, published at /docs/mcp by
> the site's build-docs.mjs. Copy agent.
>
> Content:
>
> * copy-paste config for Claude Code, Cursor, VS Code, Claude Desktop,
>   Codex and Devin
> * stdio (`hyp mcp serve`) and remote (Cloud /v1/mcp with a bearer read
>   token) options
> * how to get a read token (`hyp remote mint --scope query`)
> * replaces the four MCP setup lines in Querying
>
> Build requirements: add a docs/README.md table row and an `llms.txt`
> line, or the site build fails. Follow the docs redesign.
>
> Done when: /docs/mcp is live and each client config is tested.

**HYP-139, "MCP 1.11: New page /docs/mcp-reference, generated from tool
definitions":**

> Phase 1 docs, hypaware/docs plus a generator in the docs build. Coding
> agent.
>
> One entry per MCP tool and agent command (prompt), with its CLI equivalent
> side by side. Generate from the tool and prompt definitions so it can't
> drift from the code. Needs a docs/README.md row and an `llms.txt` line.
>
> Done when: /docs/mcp-reference is live and regenerates on each docs build.

## What this request reaches that it does not own {#reach}

- **The server.** HYP-132 and HYP-133 change `src/core/mcp/server.js`,
  which hypaware-server loads for `/v1/mcp`. A client merge reaches
  production on the next server deploy. The recommendation tools those
  prompts name are hypaware-server LLP 0596; the token `--scope query` mints
  is hypaware-server LLP 0597.
- **The mark.** HYP-133's "fix a recommendation" ends in a status mark. On
  `/v1/mcp` that mark depends on Hernan's choice in hypaware-server
  LLP 0596#mark. The client's share of the recommended option is the `write`
  auth class (LLP 0494#write-class).
- **The site.** HYP-138 and HYP-139 need a hypaware.ai `llms.txt` line and a
  site build change. Those are HYP-140's, which is held.
- **HYP-75.** HYP-135's Done-when names its pilot recipes. HYP-75 is not in
  this mission.

## Out of scope {#out-of-scope}

- Phases 2 and 3, HYP-140 and HYP-75. They appear only as seams in
  LLP 0494#seams.
- Recommendation tools served by the local stdio host. The recommendations
  live on the server; a local agent with `hyp` reaches them through the CLI
  or the remote proxy.
- Tool annotations and registry metadata (Phase 2).
