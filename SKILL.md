---
name: create-loadout-skill
description: Prepare or revise a local, partner-authored Aident Loadout Skill candidate for internal curator review. Use only when a user explicitly invokes $create-loadout-skill or asks to author, draft, adapt, or revise public guidance that depends on Aident Loadout Integrations, Actions, or Skills. Do not use for routine Loadout usage, integration creation, validation, publication, or promotion.
---

# Create A Loadout Skill Candidate

Prepare a local review candidate that Aident can validate and curate later. Do not publish it or require access to
Aident's private repositories, schemas, packager, database, or admin commands.

## Boundaries

- Treat the candidate as public, platform-owned guidance if Aident accepts it.
- Ask the user only for information that cannot be inferred safely. Use plain language, not schema terminology.
- Never infer authorization from a request to create the candidate. Obtain explicit confirmation that Aident may use
  the contributed source material before completing the handoff.
- Never include credentials, private data, binaries, scripts, remote includes, data URLs, or embedded media.
- Never call an admin command or claim that Aident validated, saved, published, or activated the candidate.
- Never invent a capability name. Copy exact canonical names from live Loadout discovery.
- Never execute a mutating Action merely to prepare or test a candidate.
- Stop if the workflow needs an unavailable capability and substituting another would materially change the goal.
- Stop if the content has no genuine Integration, Action, or Skill dependency. General information is Knowledge, which
  is outside the current Loadout Skill contract.

## 1. Define The Candidate

Establish:

- the task the Skill teaches an agent to complete;
- the intended user and outcome;
- prerequisites, confirmation points, and failure boundaries;
- the source material the user is authorized to contribute; and
- a concise display name, summary, category, English language tag, and up to ten lowercase hyphenated tags.

Ask the user to confirm explicitly that Aident may use the contributed source material. Do not require the user to
supply filenames, JSON, or capability identifiers when the agent can derive them.

Create one `SKILL.md` entrypoint. Add supporting `.md`, `.txt`, `.json`, `.yaml`, or `.yml` files only when progressive
loading makes the guidance clearer. Link every supporting file from `SKILL.md` or another reachable Markdown file.

Keep the Skill package within these limits:

- 50 files and 256 KiB total;
- 32 KiB for `SKILL.md`;
- 64 KiB for each supporting file;
- 8 path segments and 180 characters per path; and
- 100 total references, including at most 25 Skill references.

## 2. Resolve Loadout Capabilities

If the Aident CLI is unavailable or unauthenticated, follow `https://aident.ai/SETUP.md`. Search executable
capabilities and public text Skills independently:

```bash
aident capabilities search --query "<task>" --json
aident skills search --query "<related guidance>" --json
```

If either command is unavailable, follow the setup document to update the CLI and retry once. If Skill search remains
unavailable, proceed only when the candidate does not require another Skill and record the limitation in `handoff.md`.

Use:

- an Action reference for one exact operation;
- an Integration reference for provider-wide guidance; and
- a Skill reference only when the workflow genuinely depends on that public Skill.

Place a short `## Loadout capabilities` section near the top of the candidate's `SKILL.md`. Explain why and when each
capability is used, then insert its exact canonical name as a live prose tag:

```markdown
Use <action-tag>composio:gmail_tools:gmail_send_email</action-tag> to send the reviewed message.
```

The valid tags are `<action-tag>`, `<integration-tag>`, and `<skill-tag>`. Tags inside inline or fenced code are only
examples. Give every distinct live tag one matching metadata reference, and make every metadata reference appear as a
live tag.

## 3. Write Knowledge-First Instructions

Explain selection rules, required inputs, sequencing, confirmation points, expected outputs, and recovery behavior.
Keep guidance separate from execution: an agent reads the Skill, then calls a referenced Action separately through
Loadout under its existing schema, authorization, risk, billing, and confirmation controls.

Write one realistic end-to-end walkthrough that an Aident curator can execute later. Do not execute mutating Actions
during candidate preparation.

## 4. Create Candidate Metadata

Create `metadata.json` outside the Skill package:

```json
{
  "schemaVersion": "0.1.0",
  "displayName": "Launch update workflow",
  "summary": "Prepare, review, and send a launch update with Gmail.",
  "language": "en",
  "tags": ["launch", "email"],
  "category": "Productivity",
  "references": [
    {
      "type": "action",
      "name": "composio:gmail_tools:gmail_send_email",
      "required": true
    }
  ]
}
```

For a revision candidate, record that it revises an existing Skill. Do not guess its canonical name, expected revision,
or artifact digest. Leave curator-owned concurrency fields for Aident to resolve.

## 5. Prepare The Handoff

Use this layout in the current workspace or another user-approved local directory:

```text
loadout-skill-candidate/
  skill/
    SKILL.md
    <supporting files>
  metadata.json
  handoff.md
```

Keep `handoff.md` outside `skill/`. Record:

- source URLs or document names and the user's confirmation that Aident may use the material;
- the intended user and outcome;
- one end-to-end test scenario with expected behavior;
- assumptions, unresolved questions, and unavailable capabilities; and
- a manifest of candidate files.

## 6. Check The Candidate

Before handing off:

1. Confirm every local Markdown link resolves inside `skill/`.
2. Parse every JSON and YAML file.
3. Confirm live capability tags and metadata references match one-to-one.
4. Check the file, path, byte, tag, and reference limits.
5. Scan for credentials, private data, unsafe links, scripts, and unsupported embedded content.
6. Show the user the candidate summary, capability mappings, output path, and warnings.

Finish with the exact status `candidate prepared for Aident curator review`. Do not describe the candidate as
validated, saved, published, active, or available in Loadout.
