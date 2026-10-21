# 0001. Record architecture decisions

- Status: Accepted
- Date: 2026-10-21

## Context

mino-fse is both a rebuild of mino.pl and a public code sample. Reviewers, and the maintainer a few months from now, need to understand why the code looks the way it does: why a block theme, why DDEV, why a token pipeline. Knowing what the code does is not enough. Pull request descriptions capture some of this reasoning, but it is scattered and hard to find once the pull requests are merged.

## Decision

We will record every architecturally significant decision as an Architecture Decision Record (ADR). We follow the lightweight format described by [Michael Nygard](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions), extended with an "Alternatives considered" section.

- ADRs live in `docs/adr/` as `NNNN-kebab-case-title.md`, numbered sequentially. New ones start from [the template](0000-template.md).
- Each ADR has a status, a date, and the sections Context, Decision, Consequences and Alternatives considered.
- ADRs are written in English and added in the same pull request as the change they describe.
- An accepted ADR is not rewritten. When a decision changes, a new ADR supersedes the old one, and both link to each other.
- [The index](README.md) lists every ADR with its status.

A decision is architecturally significant when it is hard to reverse, affects several parts of the codebase, or chooses between credible alternatives, for example tooling, repository layout, data model or conventions.

## Consequences

- Every significant change carries a small writing cost.
- The repository explains itself: a reviewer can follow the reasoning from the index instead of reading pull request history.
- Superseded ADRs keep a record of how the project evolved, including the decisions that turned out to be wrong.

## Alternatives considered

- **Reasoning only in pull request descriptions**: no extra files, but the reasoning ends up scattered across merged pull requests and is hard to find.
- **The full [MADR](https://adr.github.io/madr/) template**: more structured (decision drivers, pros and cons per option), but heavier than a project of this size needs.
- **A wiki or an external document**: not versioned with the code and invisible in pull requests.
