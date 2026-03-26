---
name: chat-summary
description: Produces a structured summary of an agent's work at the end of a conversation, including context, architecture, design decisions, rationales, trade-offs, alternatives, validation, and follow-up actions. Use when the user asks to wrap up a session, document reasoning, or generate architecture/design notes (e.g. \"chat summary\", \"design notes\", \"ADR\", \"rationales\", \"trade-offs\", \"follow-ups\").
---

# Chat Summary

## When to Use

Use this skill at the **end of an agent conversation** when:

- The user explicitly asks for a **summary**, **design notes**, **ADR**, or **rationales/trade-offs**.
- The agent has just completed a **feature implementation**, **refactor**, **bug fix**, or **analysis** and the user wants it documented.
- You see long reasoning chains or multiple approaches explored and want to capture the final outcome and thought process.

Typical triggers include phrases like:
- \"Summarise what we did\"
- \"Write design notes\"
- \"Document the architecture/decisions\"
- \"Create an ADR\"
- \"Explain the trade-offs and alternatives\"

## Output Requirements

Save the summary as markdown file in the `docs/design-notes` directory using a descriptive filename and the structured template:

Always produce **two layers** of output:

1. **Executive summary**: 3–7 concise bullets for quick recall.
2. **Detailed sections**: Markdown-formatted sections that document the thought process.

The detailed sections **must cover at least** the following topics (combine sections only if truly overlapping for this task):

1. Change/feature context  
2. Final approach / architecture  
3. Design decisions  
4. Rationales  
5. Trade-offs  
6. Alternatives considered and reasons for rejection  
7. Follow-ups and future improvements  
8. Validation approach  

## Standard Output Template

When this skill is invoked, follow this exact structure:

```markdown
## Executive summary

- **Goal**: ...
- **Scope**: ...
- **Final approach**: ...
- **Key trade-offs**: ...
- **Validation**: ...
- **Follow-ups**: ...

## Change/feature context

- **Problem / request**: ...
- **Relevant systems / components**: ...
- **Constraints and assumptions**: ...
- **Out of scope**: ... (if applicable)

## Final approach / architecture

- **High-level design**: ...
- **Key components**: ...
- **Data flow / control flow**: ...
- **External dependencies / integrations**: ...

## Design decisions

- Decision 1: ...
- Decision 2: ...

## Rationales

- **Decision 1**: Why this was chosen over other options, including key supporting arguments.
- **Decision 2**: ...

## Trade-offs

- **Performance vs complexity**: ...
- **Flexibility vs simplicity**: ...
- **Short-term vs long-term**: ...

## Alternatives considered and reasons for rejection

- **Alternative A**: description, pros, cons, and specific reasons it was rejected.
- **Alternative B**: ...

## Validation approach

- **What was validated**: ...
- **How it was validated** (tests, manual checks, tools): ...
- **Coverage and gaps**: ...

## Follow-ups and future improvements

- **Immediate follow-ups** (short term): ...
- **Potential future enhancements** (medium/long term): ...
- **Risks to monitor**: ...
```

Adapt the bullet content to the specific conversation, but **preserve the headings** so notes are consistent across sessions.

## Instructions for the Agent

1. **Reconstruct context**
   - Scan the conversation and (if available) recent code changes to infer:
     - The original problem and scope.
     - Key constraints, assumptions, and non-goals.
     - The final approach that was implemented or recommended.

2. **Identify major decisions**
   - List 2–7 decisions that actually matter for maintainability, architecture, or user experience.
   - Prefer decisions that would be useful for:
     - Future maintainers reading the code.
     - Someone revisiting this feature months later.

3. **Surface reasoning, not step-by-step logs**
   - Do **not** restate every step of the conversation.
   - Focus on:
     - Why this approach is reasonable.
     - What was deliberately not done.
     - Important trade-offs and risks.

4. **Be explicit about uncertainty**
   - If some aspects are based on assumptions (e.g. missing requirements, unknown traffic patterns), state them clearly in:
     - \"Constraints and assumptions\"
     - \"Risks to monitor\"

5. **Keep it concise but information-dense**
   - Aim for:
     - Executive summary: <= 10 short bullets.
     - Each detailed section: 2–8 bullets.
   - Avoid redundant restatements across sections.

6. **Tailor to the work type**
   - **New feature / architecture**: Emphasise structure, data flow, integrations, and long-term implications.
   - **Refactor**: Emphasise before/after architecture, risk areas, and how behaviour is preserved.
   - **Bug fix**: Emphasise root cause, fix strategy, and regression risks.
   - **Analysis / design-only**: Emphasise options explored, recommended path, and open questions.

7. **Save the summary as markdown**
   - When file-editing tools are available (e.g. within an IDE workspace):
     - Determine the repository root.
     - If a documentation directory already exists (for example `docs/` or another clearly project-standard docs folder), save the summary markdown file there.
     - If no documentation directory is present, create a `docs/` folder at the root of the repository and save the markdown file inside it.
   - Use a clear, descriptive filename (for example including date, ticket/issue ID, and a short slug for the change), and ensure the saved content matches the **Standard Output Template**.

## Examples

### Example trigger

- User: \"/chat-summary\" or \"Can you write design notes for this change?\"
- Agent: Apply this skill and respond using the **Standard Output Template** above, filled with task-specific content.

This skill is purely instructional; it does not require or assume any specific programming language, framework, or repository layout.

