# Agent team

The following custom agent team works together in GitHub Copilot CLI (running in Codespace) to build Mona's Project Pulse dashboard.

## Orchestrator
- **Model**: Claude Opus 4.7 (copilot)
- **Location**: `.github/agents/orchestrator.agent.md`
- **Responsibility**: Coordinates Planner, Coder, and Designer agents. Breaks down complex requests into tasks, delegates work to specialists, manages dependencies, and verifies integrated results.

## Planner
- **Model**: Claude Opus 4.7 (copilot)
- **Location**: `.github/agents/planner.agent.md`
- **Responsibility**: Creates implementation plans by researching the codebase, documentation, dependencies, and edge cases. Produces practical plans with ordered steps, file assignments, and validation expectations.

## Coder
- **Model**: GPT-5.5 (copilot)
- **Location**: `.github/agents/coder.agent.md`
- **Responsibility**: Implements code-oriented tasks with clear structure, explicit errors, and testable behavior. Writes code, fixes bugs, implements logic, and creates support files like launch configurations for runnable applications.

## Designer
- **Model**: Gemini 3.1 Pro (copilot)
- **Location**: `.github/agents/designer.agent.md`
- **Responsibility**: Handles UI/UX, accessibility, information architecture, interaction flow, and visual design. Creates a polished Project Pulse dashboard with proper styling, responsiveness, and visual affordances.

## Orchestration approach
These agents are orchestrated through GitHub Copilot CLI in a Codespace, enabling collaborative AI-driven development with clear specialization, parallel execution where possible, and sequential coordination when work overlaps or has dependencies.
