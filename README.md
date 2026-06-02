# devflow-standards

Biblioteca **standalone** dos standards default de engenharia do [DevFlow](https://github.com/NEXUZ-SYS/devflow).

São ~20 standards **concern-first, warn-only** (guidance imperativa + anti-patterns) que o DevFlow shippa por padrão e injeta just-in-time por `applyTo`/task. Este repo é a **fonte viva**: o DevFlow vendoriza um snapshot em `assets/standards/` e o `/devflow update` (Step 4d) refresca via fetch https deste repo.

## Estrutura

- `std-<concern>.md` — um standard por concern (frontmatter + `## Princípios` + `## Anti-patterns`). `source: devflow-default`, `enforcement.linter: null` (warn-only).
- `MANIFEST.txt` — lista dos arquivos (consumida pelo fetch do DevFlow).

## Como o DevFlow consome

`scripts/update-default-standards.sh` faz `HEAD` em `main/MANIFEST.txt`; se vivo, busca cada `main/std-<id>.md`, valida o nome (`^std-[a-z][a-z0-9-]+\.md$`), sanitiza o corpo (SI-6) e atualiza o snapshot local. Fail-safe offline.

## Escopo

Universais (broad `applyTo`): security, runtime-validation, error-handling, test-discipline, observability, performance, documentation, code-review, grounding, naming-conventions, migration, data-modeling, api-conventions, secret-conventions, schemas, commit-hygiene.

Condicionais (applyTo estreito): accessibility, internationalization, caching, state-management.

Contracts DB-específicos (postgres/pgvector/bigquery/firebase) **não** vivem aqui — pertencem ao subsistema de stacks do DevFlow.

## Contribuir

PRs bem-vindos. Mantenha cada std concern-first (nunca lib-centric), warn-only, e dentro do orçamento (~30–80 linhas). Decisão arquitetural: ver ADR-007 no repo do DevFlow.

---
Mantido para o [DevFlow](https://github.com/NEXUZ-SYS/devflow) · MIT
