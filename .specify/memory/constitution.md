<!--
Sync Impact Report
Version change: placeholder -> 1.0.0
Modified principles: added Training-First Transparency; Security Discipline; Service-Layer Authorization; Minimal Observable Design; Spec-Driven Development
Added sections: Additional Constraints, Development Workflow
Removed sections: none
Templates requiring review: .specify/templates/plan-template.md ⚠ review for gate alignment; .specify/templates/spec-template.md ⚠ review for acceptance criteria alignment; .specify/templates/tasks-template.md ⚠ review for task organization alignment
Follow-up TODOs: none
-->

# ContosoDashboard Constitution

## Core Principles

### I. Training-First Transparency
The repository exists to teach Spec-Driven Development through a runnable, self-contained training application. All changes MUST preserve the offline, mock-auth training model and keep architecture intentionally simple. Any feature that would require paid cloud services or external dependencies MUST be documented as a training-only tradeoff.

### II. Security Discipline
Even for a training app, authorization MUST be enforced at the page, service, and data layers. Mock authentication is acceptable only for local training, and any identity or access-control change MUST clearly state its training-only status. Security headers, RBAC, and IDOR protections are mandatory for all protected workflows.

### III. Service-Layer Authorization
Business logic MUST be implemented through services that enforce user permissions, not only UI checks. Data access MUST be restricted by identity and project membership rules before returning objects. Any authorization gap discovered in service code is treated as a defect.

### IV. Minimal Observable Design
Features MUST remain simple, maintainable, and observable. Prefer explicit state, predictable service behavior, and clear user flows over complex abstractions. Debuggability in training is achieved through deterministic service methods and explicit UI state rather than hidden side effects.

### V. Spec-Driven Development
All feature work MUST use `.specify` artifacts: spec, plan, and tasks templates. Changes MUST be documented in the Spec Kit workflow before implementation. Every new feature MUST identify user stories, acceptance criteria, and measurable success conditions.

## Additional Constraints
This application is training-only; it is not production software. Do not add dependencies that require a paid cloud account or external service unless the repository clearly separates them behind an abstraction. The app MUST remain runnable locally with EF Core and SQL Server/LocalDB. Preserve the current mock authentication pattern for training and clearly label any future identity changes as “training implementation only.”

## Development Workflow
Start by filling `specs/[feature]/spec.md` and `plan.md` using the `.specify/templates` guidance. Use `tasks.md` to organize implementation by independent user story, with explicit file paths and priority tags. Every PR MUST reference relevant spec/template artifacts and explain how it preserves training constraints. Reviewers MUST verify authorization at both the UI and service layers for each feature. When updating data models, also update the feature spec and service contract documentation.

## Governance
The constitution supersedes informal practices for this repository. Amendments require updating this file, incrementing `Version`, and documenting the change in the Sync Impact Report comment. All PRs MUST include a constitution compliance statement describing which principles were followed. The project’s training and security constraints MUST be reviewed on each major feature branch.

**Version**: 1.0.0 | **Ratified**: 2026-06-01 | **Last Amended**: 2026-06-01
