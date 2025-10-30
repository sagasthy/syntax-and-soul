## Modes

- Ask, Edit & Agent modes

<br> **Note:** Always review the changes before accepting them into the codebase

  

## Models

- Try deeper “thinking” models (e.g., Claude Sonnet 4.5 or GPT-5) for analysis-heavy prompts

- Use GPT-4.1 for faster, surface-level tasks.

  

## Context

- @workspace (VS Code)

- @project (IntelliJ)

- @solution (Visual Studio)

  

## Custom Instructions

- Repository

- .github/copilot-instructions.md

- .github/instructions/**/NAME.instructions.md

- AGENTS.md

- Organization (not available outside Enterprise plan)

- Personal (not available outside GitHub)

- Examples:

1. Always respond in Spanish.

2. Your style is a helpful colleague, minimize explanations but provide enough context to understand the code.

3. Always provide examples in TypeScript.

  

## Generate Commit Messages

- Eclipse, IntelliJ, VS Code, Visual Studio has a commit helper with respective commit tools, which will generate a commit message for the change set.

  

## Commit Instructions

- .github/git-commit-instructions.md

- Eg.

when generating commit messages, extract the JIRA ticket id from the current branch, if present, and use it as a prefix for the summary of the commit message. JIRA Ticket ids are of the form: "ICSMFRSMAT-XXXXX". The body of the commit message should be a "change log" style summary of the changes. The commit message should look like this:

JIRA-TICKET: SUMMARY

- CHANGE description 1

- CHANGE description 2

  

## Code Review (not available in Eclipse)

- **VS Code** 
	- Search for "Chat: Review" in Command Palette for open file review
	- For review of all uncommitted changes, go to Source Control and look for <> Copilot Review Tool.
- **IntelliJ:** "Review Code Changes" option in Commit tool  

## Common Tasks/Use-cases

- Code refactoring
- SQL explain plan, SP analysis, etc.
- Explain complicated code/applications
- Identify OWASP 10 issues in the code=
- Use it for creating issue descriptions and details
- Use it to generate a PR summary
- Generate markdown tables
- Mermaid.js diagram generation: Generate technical diagrams as code (sequence diagrams, flowcharts, data flow diagrams) from existing implementations for better documentation

## Custom Chatmodes

- .github/chatmodes/<Chat Mode Name>.chatmode.md

- https://docs.github.com/en/copilot/how-tos/configure-custom-instructions/add-repository-instructions?tool=jetbrains

  

## Copilot CLI