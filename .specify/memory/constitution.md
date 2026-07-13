<!--
Sync Impact Report
- Version change: template placeholder -> 1.0.0
- Modified principles: none -> I. Training-First and Offline-Capable, II. Security-by-Design, III. Test-First and Evidence-Based Change, IV. Simplicity Over Abstraction, V. Documentation and Traceability
- Added sections: Additional Constraints, Development Workflow
- Removed sections: none
- Templates requiring updates: ✅ .specify/templates/plan-template.md, ✅ .specify/templates/spec-template.md, ✅ .specify/templates/tasks-template.md
- Follow-up TODOs: none
-->

# ContosoDashboard Constitution

## Core Principles

### I. Training-First and Offline-Capable
ContosoDashboard MUST remain usable in an offline classroom environment. New features MUST work without external services by default, and any future cloud integration MUST be introduced through abstractions and configuration rather than embedded in business logic. This is non-negotiable because the repository is a training scaffold intended to run without Azure subscriptions or internet access.

### II. Security-by-Design
Every protected feature MUST enforce authentication and authorization at both the UI and service layers. The application MUST prevent IDOR, preserve per-user data isolation, and avoid insecure defaults such as trusting client-side state or weakening access checks. This is non-negotiable because the project exists to teach secure application patterns.

### III. Test-First and Evidence-Based Change
Behavioral changes MUST be accompanied by a concrete validation scenario before implementation, and the implementation MUST be verified by running the relevant app flow or automated test. Changes that affect security, data access, navigation, or user-visible behavior MUST be validated explicitly. This is non-negotiable because the repository is educational and regressions should be obvious and reproducible.

### IV. Simplicity Over Abstraction
The team MUST prefer the smallest change that satisfies the requirement, reusing existing models, services, and pages where they already provide the needed behavior. New abstractions, layers, or frameworks MUST be justified by a clear need and documented in the change. This is non-negotiable because the app is intentionally simple and should stay readable for learners.

### V. Documentation and Traceability
User-facing changes MUST be reflected in the README or stakeholder documentation, and the intent of each change MUST be understandable from the code and supporting docs. Features that introduce new workflows or constraints MUST include enough context for future contributors to understand why the behavior exists. This is non-negotiable because the repository is used as a teaching artifact.

## Additional Constraints
The default implementation MUST remain aligned with the existing ASP.NET Core 8 / Blazor Server architecture, EF Core data access, and SQL Server LocalDB development workflow. Business logic MUST live in services rather than being embedded directly in Razor pages, and new features MUST preserve the current training-only authentication model unless a documented migration path is provided. The baseline experience MUST continue to function without external services, and any new dependency or external integration MUST be clearly justified and isolated behind an interface.

## Development Workflow
All work MUST start from a clear requirement, user scenario, or acceptance criterion. Changes affecting data access, authentication, authorization, or user-visible behavior MUST include an explicit validation step before completion, and the final change MUST preserve the constitution's security, offline, and documentation requirements. Pull requests MUST describe the behavior being added, the verification performed, and any trade-offs or follow-up work.

## Governance
This constitution supersedes other local practices for this repository whenever there is a conflict. Amendments MUST include a version bump, a rationale for the change, and a review of impact on training suitability, security posture, and existing workflow. The project maintainer or designated reviewer MUST confirm compliance before a change is accepted, and any deviation from these principles MUST be documented and explicitly justified.

**Version**: 1.0.0 | **Ratified**: 2026-07-13 | **Last Amended**: 2026-07-13
