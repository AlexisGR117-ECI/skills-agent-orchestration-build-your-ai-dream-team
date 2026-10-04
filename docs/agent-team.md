# Agent team

Mona's Project Pulse dashboard will be built by a specialist team orchestrated from **GitHub Copilot CLI in a Codespace**. The Orchestrator coordinates the work, assigns non-overlapping file scopes, and integrates the result without making implementation changes itself.

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 4.7 (copilot) | Converts the plan into dependency-aware phases, delegates scoped work to specialists, coordinates parallel and sequential execution, and verifies the integrated dashboard. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 4.7 (copilot) | Researches the repository and relevant documentation, identifies requirements, dependencies, edge cases, and risks, then produces the implementation plan and file assignments. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements application behavior with clear, deterministic, testable code; validates changes; and, when assigned, adds Project Pulse runnable-app support such as `.vscode/launch.json`. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.1 Pro (copilot) | Defines and implements the dashboard's UI/UX, accessibility, information hierarchy, interaction flow, responsive behavior, and visual polish. | `.github/agents/designer.agent.md` |

All agents leave staging, commits, and pushes under the learner's control through Copilot CLI prompts.
